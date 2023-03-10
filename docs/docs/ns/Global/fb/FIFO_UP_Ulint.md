# QueueLib | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Function block FIFO_UP_Ulint  

Unordered Priority FIFO buffer. Allows Enqueing if buffer is not full. Dequeues the highest priority elements first.  

### Interface  

| Direction | Name | Type | Attribute | Initial Value | Documentation |
| --------- | ---- | ---- | --------- | ------------- | ------------- |
| VAR_INPUT | Buffer | POINTER TO dtFIFO_P_Element_ULINT | constant | 0 | Externally allocated buffer. Must be in format ARRAY[0..N]. ! Block doesn't check for Null pointer |  
| VAR_INPUT | Size | UINT | constant | 0 | Size/Max elements of the buffer |  
| VAR_OUTPUT | NrElements | UINT |  | 0 | Number of elements in the FIFO |  
| VAR | vLast | INT |  | -1 | Position of the last item |  

Developed entirely with methods  

## Metrics  

- VAR_INPUT : 2
- VAR_OUTPUT : 1
- VAR : 1

| Actions | Methods | Lines of code | Lines of comments | Lines in total | Maintainable size |
| ------- | ------- | ------------- | ----------------- | -------------- | ----------------- |
| 0 | 5 | 38 |10 |58 | 45 |



## Methods  
- Dequeue
- Enqueue
- isEmpty
- isFull
- Peek

### Method Dequeue : BOOL  

Returns the next FIFO element stored in the buffer  

#### Method Interface  

| Direction | Name | Type | Attribute | Initial Value | Documentation |
| --------- | ---- | ---- | --------- | ------------- | ------------- |
| VAR_OUTPUT | Value | ULINT |  |  | Return value |  
| VAR_OUTPUT | Priority | INT |  |  | Return priority |  
| VAR | vHighestPrioIdx | INT |  |  | Highest priority index |  
| VAR | i | INT |  |  | Iterator variable |  


### Method Enqueue : BOOL  

Inserts new value into the FIFO buffer  

#### Method Interface  

| Direction | Name | Type | Attribute | Initial Value | Documentation |
| --------- | ---- | ---- | --------- | ------------- | ------------- |
| VAR_INPUT | Value | ULINT |  |  | Value to add to queue |  
| VAR_INPUT | Priority | INT |  |  | Return priority |  


### Method isEmpty : BOOL  

Returns TRUE if FIFO is empty. Reading not allowed  

#### Method Interface  

| Direction | Name | Type | Attribute | Initial Value | Documentation |
| --------- | ---- | ---- | --------- | ------------- | ------------- |


### Method isFull : BOOL  

Returns TRUE if FIFO is full. Inserting not allowed  

#### Method Interface  

| Direction | Name | Type | Attribute | Initial Value | Documentation |
| --------- | ---- | ---- | --------- | ------------- | ------------- |


### Method Peek : UINT  

Returns the index of the next element to Dequeue. Output paramter holds the element without removing it from the Buffer  

#### Method Interface  

| Direction | Name | Type | Attribute | Initial Value | Documentation |
| --------- | ---- | ---- | --------- | ------------- | ------------- |
| VAR_OUTPUT | Value | ULINT |  |  | Return value |  
| VAR_OUTPUT | Priority | INT |  |  | Return priority |  
| VAR | maxPrio | INT |  | -32768 | Current highest priority value |  
| VAR | i | UINT |  |  | Iterator variable |  



---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index.md
[NAMESPACES]: ../../nsList.md
[METRICS]: ../../../metrics.md
[BACK]: ../nsMain.md
