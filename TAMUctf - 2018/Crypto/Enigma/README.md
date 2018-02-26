Enigma
===========
**Category**: Crypto  **Points**: 75

Challenge Description
------
```
Agent Bob is working at a law firm and loved history. He could not figure out how to crack the python enigma emulator, even though it was broken before. Since he could not crack the python enigma emulator, he decided that it would be ok to to encrypt all his emails with the emulator before sending them. NOT A GOOD IDEA!! Can you crack it?

You have intercepted two messages. The first is

    1. "IPUXZGICZWASMJFGLFVIHCAYEGT". The second is "LTHCHHBUZODFLJOAFNNAEONXPLDJQVJCZPGAVOLN"
    2. You have decrpted the first message and found that it is "HOWDYAGGIESTHEWEATHERISFINE"
    3. Assume that "I II III" are rotors.
    4. Plugboard is "AV BS CG DL FU HZ IN KM OW RX"

What is the flag? (The flag is in a slightly different format.)
```

Solutions
--------------

I used the py-enigma emulator as suggested, there are two things that needs brute forcing

1. Ring settings
2. Rotor Positions

Below is my code (not elegant but it does the job...)

```
from enigma.machine import EnigmaMachine
import sys

msg1 = "IPUXZGICZWASMJFGLFVIHCAYEGT"
msg2 = "LTHCHHBUZODFLJOAFNNAEONXPLDJQVJCZPGAVOLN"
msg1_d = "HOWDYAGGIESTHEWEATHERISFINE"
msg2_d = ""


# Brute force the rings
for ring1 in range(65, 91):
  for ring2 in range(65, 91):
    for ring3 in range(65, 91):
      machine = EnigmaMachine.from_key_sheet(
      rotors='I II III',
      reflector='B',
      ring_settings=chr(ring1)+' '+chr(ring2)+' '+chr(ring3),
      plugboard_settings='AV BS CG DL FU HZ IN KM OW RX')
      # Brute force rotor initial positions     
      for l in range(65, 91):
        for m in range(65, 91):
          for n in range(65, 91):
            rotors_pos = chr(l)+chr(m)+chr(n)
            machine.set_display(rotors_pos)
            c = machine.process_text(msg1)
            print c + "\r", 
            if c == msg1_d:
              print 'Rings: ' + chr(ring1)+' '+chr(ring2)+' '+chr(ring3)
              print 'Rotor Positions: ' + machine.get_display()
              # Necessary to reset the machine otherwise it would decrypt differently
              machine.set_display(rotors_pos)
              print 'Decrypted msg2: ' + machine.process_text(msg2)
              sys.exit()
```

Flag: PASSWORDISGIGEMXHISTORYROCKSLEARNCRYPTOX
