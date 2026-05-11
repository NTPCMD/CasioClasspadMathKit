# Minimal FX-CP400 Runnable Template Set

This is a conservative minimal structure intended for ClassPad II FX-CP400 import testing.

## 1) `MN_DEMO`

```text
Lbl 1
Menu "DEMO",
"HELLO",A,
"CALC",B,
"EXIT",Z

Lbl A
Prog "DM_HELLO"
Goto 1

Lbl B
Prog "DM_CALC"
Goto 1

Lbl Z
Return
```

## 2) `DM_HELLO`

```text
ClrText
Locate 1,1,"HELLO WORLD"
Pause
Return
```

## 3) `DM_CALC`

```text
ClrText
Input "X?",X
X^2→Y
Locate 1,2,"Y="
Locate 3,2,Y
Pause
Return
```

## Verification procedure

1. Paste these into ClassPad Manager program editor.
2. Run each program in emulator.
3. Confirm menu navigation, input, output, and return behavior.
4. Export/import on hardware and re-run.

> In this repository environment we cannot execute ClassPad firmware directly, so final run confirmation must be done in emulator/device QA.
