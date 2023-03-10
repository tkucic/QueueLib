# QueueLib | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Program FIFO_OP_ManTest  

```pascal
//Manual tests for function block FIFO_OP_Int  
INTERFACE
    VAR 
        fifo : FIFO_OP_Int;
        vFifoBuffer : ARRAY[0..4] OF dtFIFO_P_Element_Int;
        fifoa : FIFO_OP_Int;
        vFifoaBuffer : ARRAY[0..4] OF dtFIFO_P_Element_Int;
        vEnq : BOOL;
        vEnqVal : INT;
        vEnqPrio : INT;
        vEnqRet : BOOL;
        vDeq : BOOL;
        vDeqVal : INT;
        vDeqPrio : INT;
        vDeqRet : BOOL;
        isFull : BOOL;
        isEmpty : BOOL;
        vCounter : INT;
        vAutoDeqValue : INT;
        vAutoDeqPrio : INT;
    END_VAR
END_INTERFACE
PROGRAM FIFO_OP_ManTest:
    (*Initialize the fifos*)
    fifo.Buffer := ADR(vFifoBuffer);
    fifo.Size   := SIZEOF(vFifoBuffer)/SIZEOF(vFifoBuffer[0]);

    fifoa.Buffer := ADR(vFifoaBuffer);
    fifoa.Size   := SIZEOF(vFifoaBuffer)/SIZEOF(vFifoaBuffer[0]);

    (*Manual testing enqueing and dequeing*)
    IF vEnq THEN
        vEnq := FALSE;
        vEnqRet := fifo.Enqueue(vEnqVal, vEnqPrio);
    END_IF

    IF vDeq THEN
        vDeq := FALSE;
        vDeqRet := fifo.Dequeue(Value=>vDeqVal, Priority => vDeqPrio);
    END_IF
    isFull := fifo.isFull();
    isEmpty := fifo.isEmpty();
    fifo.NrElements;

    (*Automatic testing*)
    WHILE NOT fifoa.isFull() DO
        vCounter := vCounter + 1;
        fifoa.Enqueue(Value := vCounter, Priority := vCounter -1);
    END_WHILE

    IF NOT fifoa.isEmpty() THEN
        fifoa.Dequeue(Value=>vAutoDeqValue, Priority => vAutoDeqPrio);
    END_IF
    IF NOT fifoa.isEmpty() THEN
        fifoa.Dequeue(Value=>vAutoDeqValue, Priority => vAutoDeqPrio);
    END_IF
    fifoa.NrElements;
END_PROGRAM
```

## Metrics  

- VAR : 17

| Actions | Methods | Lines of code | Lines of comments | Lines in total | Maintainable size |
| ------- | ------- | ------------- | ----------------- | -------------- | ----------------- |
| 0 | 0 | 26 |3 |34 | 43 |

---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index_st.md
[NAMESPACES]: ../../nsList_st.md
[METRICS]: ../../../metrics_st.md
[BACK]: ../nsMain_st.md
