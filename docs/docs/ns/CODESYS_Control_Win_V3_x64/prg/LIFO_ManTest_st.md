# QueueLib | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Program LIFO_ManTest  

```pascal
//Manual tests for function block LIFO_Int  
INTERFACE
    VAR 
        lifo : LIFO_Int;
        lifoa : LIFO_Int;
        vLifoBuffer : ARRAY[0..20] OF INT;
        vLifoaBuffer : ARRAY[0..20] OF INT;
        vEnq : BOOL;
        vEnqVal : INT;
        vEnqRet : BOOL;
        vDeq : BOOL;
        vDeqVal : INT;
        vDeqRet : BOOL;
        isFull : BOOL;
        isEmpty : BOOL;
        vCounter : INT;
        vAutoDeq : INT;
    END_VAR
END_INTERFACE
PROGRAM LIFO_ManTest:
    (*Initialize the fifos*)
    lifo.Buffer := ADR(vLifoBuffer);
    lifo.Size   := SIZEOF(vLifoBuffer)/SIZEOF(vLifoBuffer[0]);

    lifoa.Buffer := ADR(vLifoaBuffer);
    lifoa.Size   := SIZEOF(vLifoaBuffer)/SIZEOF(vLifoaBuffer[0]);

    (*Manual testing pushing and popping*)
    IF vEnq THEN
        vEnq := FALSE;
        vEnqRet := lifo.Push(vEnqVal);
    END_IF

    IF vDeq THEN
        vDeq := FALSE;
        vDeqRet := lifo.Pop(Value=>vDeqVal);
    END_IF
    isFull := lifo.isFull();
    isEmpty := lifo.isEmpty();
    lifo.NrElements;

    (*Automatic testing*)
    WHILE NOT lifoa.isFull() DO
        vCounter := vCounter + 1;
        lifoa.Push(vCounter);
    END_WHILE

    IF NOT lifoa.isEmpty() THEN
        lifoa.Pop(Value=>vAutoDeq);
    END_IF
    lifoa.NrElements;

END_PROGRAM
```

## Metrics  

- VAR : 14

| Actions | Methods | Lines of code | Lines of comments | Lines in total | Maintainable size |
| ------- | ------- | ------------- | ----------------- | -------------- | ----------------- |
| 0 | 0 | 23 |3 |31 | 37 |

---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index_st.md
[NAMESPACES]: ../../nsList_st.md
[METRICS]: ../../../metrics_st.md
[BACK]: ../nsMain_st.md
