# QueueLib | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Program FIFO_S_UnitTest  

```pascal
//Automated unit tests for function block FIFO_S  
INTERFACE
    VAR 
        vRunTests : BOOL := FALSE; (*Tap to TRUE, to run test*)
        vReturnsTrueOnSuccEnqOk : BOOL := FALSE; (*Results*)
        vFillsThreeValuesOk : BOOL := FALSE;
        vReportsFullOk : BOOL := FALSE;
        vReturnsFalseOnUnSuccEnqOk : BOOL := FALSE;
        vReturnsTrueOnSuccDeqOk : BOOL := FALSE;
        vDequeue1Ok : BOOL := FALSE;
        vDequeue2Ok : BOOL := FALSE;
        vCountingWorks : BOOL := FALSE;
        vReportsEmptyOk : BOOL := FALSE;
        vReturnsTrueOnUnSuccDeqOk : BOOL := FALSE;
        vFifoFullPrevent1Ok : BOOL := FALSE;
        vFifoFullPrevent2Ok : BOOL := FALSE;
        vFifoFullPrevent3Ok : BOOL := FALSE;
        vPeekOk : BOOL := FALSE;
        v16bHandlingOk : BOOL := FALSE;
        fifo : FIFO_S_16b;
        vFifoBuffer : ARRAY[0..5] OF INT;
        vTempWord : WORD;
        vTempWordArr : ARRAY[0..3] OF WORD;
        vTempIdx : UINT;
    END_VAR
END_INTERFACE
PROGRAM FIFO_S_UnitTest:
    (*Init FIFO*)
    fifo.FB_Init(bInitRetains := TRUE, bInCopyCode := TRUE); //Implicit function block init, curtesy of codesys. To init function block on every run
    SysMemSet(ADR(vFifoBuffer), 0, SIZEOF(vFifoBuffer));
    fifo.Buffer := ADR(vFifoBuffer);
    fifo.Size   := SIZEOF(vFifoBuffer)/SIZEOF(INT);
    (***List of tests***)
    (***Test1: Fifo returns TRUE on successfull enqueue***)
    vReturnsTrueOnSuccEnqOk := fifo.Enqueue(1);

    (***Test2: Fifo fills three values correctly***)
    fifo.Enqueue(2);
    fifo.Enqueue(3);

    vFillsThreeValuesOk := vFifoBuffer[0] = 1 AND vFifoBuffer[1] = 2 AND vFifoBuffer[2] = 3;

    (***Test3: Fifo reports FULL when full***)
    fifo.Enqueue(4);
    fifo.Enqueue(5);
    fifo.Enqueue(6);

    vReportsFullOk := fifo.isFull();

    (***Test4: Fifo reports FALSE on unsuccesfull enqueue***)
    vReturnsFalseOnUnSuccEnqOk := NOT fifo.Enqueue(7);

    (***Test5: Fifo returns TRUE on successfull dequeue***)
    vReturnsTrueOnSuccDeqOk := fifo.Dequeue(Value => vTempWord);

    (***Test6: Fifo dequeues values correctly***)
    fifo.Dequeue(Value => vTempWord);
    vDequeue1Ok := vTempWord = 2;
    fifo.Dequeue(Value => vTempWord);
    vDequeue2Ok := vTempWord = 3;

    (***Test7: Fifo can count correctly***)
    vCountingWorks := fifo.NrElements = 3; //At this point in the tests, there should be 3 elements

    (***Test8: Fifo reports EMPTY when EMPTY***)
    fifo.Dequeue(Value => vTempWord); //Returns 4
    fifo.Dequeue(Value => vTempWord); //Returns 5
    fifo.Dequeue(Value => vTempWord); //Returns 6
    vReportsEmptyOk := fifo.isEmpty();

    (***Test9: Fifo reports FALSE on unsuccesfull dequeue***)
    vReturnsTrueOnUnSuccDeqOk := NOT fifo.Dequeue();

    (***Test10: Fifo prevents enqueue if its full***)
    fifo.Enqueue(1);
    fifo.Enqueue(2);
    fifo.Enqueue(3);
    fifo.Dequeue(Value => vTempWord); //Returns 1
    fifo.Enqueue(4);
    fifo.Enqueue(5);
    fifo.Enqueue(6); //Here it is full, even though first array index is free
    fifo.Enqueue(7); //Writing fails as it is full
    vFifoFullPrevent1Ok := fifo.isFull() AND NOT fifo.Enqueue(8);

    (***Test11: Able to enqueue again only after empty flag***)
    fifo.Dequeue(Value => vTempWord); //4 elems remaining
    fifo.Dequeue(Value => vTempWord); //3 elems remaining
    fifo.Dequeue(Value => vTempWord); //2 elems remaining
    vFifoFullPrevent2Ok := fifo.isFull() AND NOT fifo.Enqueue(8); //Test pass if it still reports full and enqueue fails

    fifo.Dequeue(Value => vTempWord); //1 elems remaining
    fifo.Dequeue(Value => vTempWord); //0 elems remaining
    vFifoFullPrevent3Ok := NOT fifo.isFull() AND fifo.Enqueue(8); //Test pass if it reports empty and enqueue goes trough

    (***Test12: Peek functionality: Should return the next element but not remove it, it should report correct index of the buffer***)
    fifo.Enqueue(9);
    vTempIdx := fifo.Peek(Value => vTempWord);
    vPeekOk := fifo.NrElements = 2 AND vTempWord = 8 AND vTempIdx = 0;

    (***Test13: 16bit handling: Multiple types should be returned***)
    WHILE NOT fifo.isEmpty() DO
        fifo.Dequeue();
    END_WHILE
    fifo.Enqueue(INT#1);
    fifo.Enqueue(INT#-1);
    fifo.Enqueue(UINT#3);
    fifo.Enqueue(WORD#4);

    fifo.Dequeue(Value => vTempWordArr[0]); //int 1
    fifo.Dequeue(Value => vTempWordArr[1]); //int -1
    fifo.Dequeue(Value => vTempWordArr[2]); //uint 3
    fifo.Dequeue(Value => vTempWordArr[3]); //word 4

    v16bHandlingOk := WORD_TO_INT(vTempWordArr[0]) = 1
                  AND WORD_TO_INT(vTempWordArr[1]) = -1
                  AND WORD_TO_UINT(vTempWordArr[2]) = 3
                  AND vTempWordArr[3] = 4;
                  
    vRunTests := FALSE;
END_PROGRAM
```

## Metrics  

- VAR : 21

| Actions | Methods | Lines of code | Lines of comments | Lines in total | Maintainable size |
| ------- | ------- | ------------- | ----------------- | -------------- | ----------------- |
| 0 | 0 | 59 |15 |92 | 80 |

---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index_st.md
[NAMESPACES]: ../../nsList_st.md
[METRICS]: ../../../metrics_st.md
[BACK]: ../nsMain_st.md
