# Chameleon Mini RevG - Chamterminal.py how to

Lets look at some commands using chamterminal.py [found here](https://github.com/salmg/ChameleonMini/blob/77d34a34bd761f4c9dfa663a211b0ba00935d879/Software/chamterminal.py)

List devices :
```bash
 Chameleon> shell ls /dev/ttyA*
 >Running shell command: ls /dev/ttyA*
/dev/ttyACM0
 Chameleon> shell ls /dev/ttyU*
 >Running shell command: ls /dev/ttyU*
/dev/ttyUSB0
```

Setup port:
```bash
Chameleon> port /dev/ttyACM0
>Setting Chameleon-mini Device: /dev/ttyACM0
```

Config setting options:
```bash
Chameleon> config ?
Possible configurations: NONE, MF_ULTRALIGHT, MF_ULTRALIGHT_EV1_80B, MF_ULTRALIGHT_EV1_164B, MF_CLASSIC_1K, MF_CLASSIC_1K_7B, MF_CLASSIC_4K, MF_CLASSIC_4K_7B, ISO14443A_SNIFF, ISO14443A_READER
```

Setup config to read ISO14443A and getuid:
```bash
Chameleon> config ISO14443A_READER
Configuration has been changed to ISO14443A_READER
Chameleon> getuid
DEADBEEF

```

Setup config and getuid:
```bash
Chameleon> config
Current configuration: MF_CLASSIC_1K
Chameleon> uid DEADBEEF
UID has been changed to DEADBEEF
Chameleon> uid
DEADBEEF
```
