---
hide:
  - navigation
  # - toc
---
# mSim: Simulation framework

Last update: July 29th, 2024

## **Introduction**

The following post describes mSim, a simulation framework used throughout this blog. It implements provides simulation capabilities similar to those offered by the simulink environment. It is, however, entirely programmatic. 

This environment was develop for the following reasons:

* Simulink restriction:
	- Home license limits number of blocks to approximately a 1000 blocks (when this project was originally posted in 2024);
	- I wouldn't be able to develop more sophisticated projects with this constraint;
* Flexibility:
	- Possibility to create/destroy objects dynamically (E.g. a given object can emerge during a simulation and disapper when it is not longer needed);
	- Integrate simulation environment with more sophisticated tools (E.g. Machine learning, sensor fusion, etc.)
* Programming language:
	- Seemslessly integrate the same framework in other programming languages;
* Personal interest:
	- Use the simulation framework as a platform to develop my programming skills.

## **Requirements**

The mSim framework was developed with the following requirements in mind:

### Basic types:

1. The framework shall offer the same basic blocks as *Simulink*:
	- *Inport/Outport*, *Constant*, *Gain*, *Unit Delay*, *Product*, *Sum*, *Integrator*, *Transfer function*, *State space*, *Logical gate*, *Rational*, *Switch*, *Rate limiter*, etc.
* The framework shall offer *logical*, *int32*, and *double* as basic data types;
* The framework shall offer a User defined block:
	- the user will provide a function with $u_r$ inputs and $y_n$ outputs, where $r,n$ are integers numbers greater than or equal to zero;
	- each function input will have one of the basic data types (boolean, int32, double);
	- the inports and outputs will be created automatically and named as $u_1,u_2,...,u_r$, $y_1,y_2,...,y_n$

### Connectivity

1. The framework shall offer a helper method to connect individually the inports of one block to the Outport of another block (similar to connection line in Simulink);
* The framework shall allow multiple Inports to be connected to be connected to each Outport;

### Simulation

1. The framework shall perform the simulation in two steps:
	- Output: Ask each block to calculate its outputs; 
	- Update: Ask each block to update its states for the next execution;
* The framework shall allow the user do define which integration method will be used;
* The framework will offer a 500Hz top-level execution rate:
	- The user is able to down-sample to other frequencies multiples of the same sample time (E.g. 100Hz, 50Hz, etc.)
* Each block shall call output/update methods of its respective sub-blocks;	
* The framework shall allow the user do define the execution order;
* The framework shall allow the user to explicity define the Initial State;
* The framework shall offer a discrete solver with not continuous states;

## **Architecture**

### Ports:

Inports and Outports are specializations of the *Port class*. The primary difference between these two types is the fact that *Inports* do not have a method to set its internal value. Its implementation pulls the 

``` mermaid
classDiagram
    class Port{
		<<Abstract>>
		-String name
		-String dataType
		-Block parent
		+getValue()
		+connectTo(Port source)
	}
	class Inport
	class Outport{
	-DataType value
	+setValue()
	}
	Port <|-- Inport
	Port <|-- Outport
```

Suppose we want to connect *Inport [inA]* of subsystem A to *Outport [outB]* of subsystem B. This is the proposed syntax: 
``` matlab  
inA = mSim.block.Inport('inA',... name
                        'double',... type
                        systemA); % parent

outB = mSim.block.Outport('outB',... name
                          'double',... type
                          systemA); % parent
						  
% Connect [inport A] to [outport B]:
inA.connectTo(outB);
```

### Blocks






``` mermaid
---
title: Block
---
classDiagram
    class Block{
		<<Abstract>>
		+String name
		+String dataType
		+String blockType
		+Port inports
		+Port outports
		+State states
		+Block subBlocks
		+Block parent
		+output()*
		+update()*
		+getOutport(String name)
		+setOutport(String name,DataType value)
		+getInport(String name)
		+connect(String portName,Port source)
	}
	style Block fill:#FFFDFE,stroke:#031E49
```


``` mermaid
---
title: Block
---
classDiagram
    class Block{
		<<Abstract>>
		+String name
		+String dataType
		+String blockType
		+Port inports
		+Port outports
		+State states
		+Block subBlocks
		+Block parent
		+output()*
		+update()*
		+getOutport(String name)
		+setOutport(String name,DataType value)
		+getInport(String name)
		+connect(String portName,Port source)
	}
	style Block fill:#FFFDFE,stroke:#031E49
```


## Testing:

## Example 1:

