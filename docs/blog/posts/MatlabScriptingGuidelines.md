---
date:
  created: 2024-06-17
readtime: 15  
---

# Matlab scripting guidelines

## Introduction

This style guide introduces the preferred coding conventions for Matlab scripting used by this blog. 

<!-- more -->

## Code layout

**Maximum line length:**

* Avoid exceeding a maximum number of 79 characters;
* Consider using line continuation for long command lines;

**Indentation:**

* Use 4 spaces per indentation level

``` matlab title="Indentation (Example 1)" 
[out1, out2] = function myFunction(argA, argB, argC)
    if(argA)
	    out1 = argB;
		out2 = argC;
	end
end
```

If the function call results in a long statement, consider aligning inputs/outputs arguments to avoid exceeding 79 characters:

``` matlab title="Indentation (Example 2)" 
foo = funcWithVeryLongName(argA,...
						   argB,...
						   argC);
```

``` matlab title="Indentation (Example 3)" 
[outLongVar1,...
        out2,...
    outName3] = funcWithVeryLongName(argA,...
   								     argB,...
								     argC);
```

If the statement is very long, consider aligning operators to avoid exceeding 79 characters:

``` matlab title="Indentation (Example 4)" 
result = operatorA * (operatorB + ...
   					  operatorC + ...
					  operatorD);
```

**Blank lines**

* Use blank lines in functions, sparingly, to indicate logical sections.

**White space**

* General rule: After commas, around operators.

**Operator precedence**

* Consider using parenthesis to organize logical statements.

**Imports**

Sometimes function calls can get very long, especially if the function is within a namespace:

``` matlab title="Imports (Example 1)" 
[out1, out2] = namespaceA.subNamespace.functionName(argA,...
													argB);
```

If that is the case, consider importing it in a separate line;
Make the import close to where it is used;

``` matlab title="Imports (Example 2)" 
import namespaceA.subNamespace.functionName
[out1, out2] = functionName(argA,argB);
```

**Comments**

* Use comments to explain "why", not "how";
* Make "what" and "how" explicit using the code itself;
* Use inline comments sparingly:
	- They are distracting - but sometimes necessary;
	- Align comments to improve readability;
* The best comment is a code that explains itself;
* Use %% to group logical section;

``` matlab title="Comments (Could be better)" 
a = 2.0; % Acceleration in m/s2
b = 1.0; % Mass in kg
m = b * a; % Force in Newtons
n = a^3 + b^2 + c; % Random equation
```

``` matlab title="Comments (More appropriate)" 
% Force using Second Newton's law:
force_N = mass_kg * accel_mps2;
```

## Naming convention

**Style**

* Constants:
	- UPPER_CASE_WITH_UNDERSCORE
* Functions (camelCase):
	- getNumberPages
* Classes (CamelCase):
	- TestCaseWrapper
* Objects(CamelCase):
	- MyObject
* Packages:
	- packagesname
* Engineering variables:
	- varName_unit
	
**Units**
	
Indicate units explicity:

* force_N;
* mass_kg;
* accel_mps2;
* vel_mps;
* dist_m;	
	
Used more often:

| Unit            | Suffix |
|:---------------:|:-------:|
|degree           | _deg    |
|degree per second| _degs   |
|feet             | _ft     |
|hertz            | _hz     |
|kilograms        | _kg     |
|knot             | _kt     |
|meter            | _m      |
|$m/s$            | _mps    |   
|$m/s^2$          | _mps2   | 
|newton           | _N      |
|pound            | _lb     |
|radian           | _rad    |
|radian per second| _rads   |
|second           | _s      |

**Iterators**

Name iterators according to what you are iterating over:

* E.g.: iSpeed, iAlt

``` matlab title="Iterators" 
for iSpeed = 1 : numel(speedMatrix)
	# Do your thing
end
```

**Descriptive names**

Use name to express meaning:

* forceBodyAxis_N

Avoid excessively long names:

* saturationUpperBoundary_deg

How long should a name be:

* As long as necessary to immediately recognize what the variable is about:
* However:
	- Short names are more appropriate for small scopes
	- For a large scope, consider a more descriptive name (potentially longer)
	
## Function arguments:

**Description**

``` matlab title="Function arguments" 
function force_N = getForce(mass_kg, accel_mps2)
% [Description]:
%	- Calculates resulting force using Second Newton's law.
% [Inputs]:
%	- mass_kg: Body Mass
%	- accel_mps2: Body acceleration
% [Outputs]:
%	- force_N: resulting Force

force_N = mass_kg .* accel_mps2;

end
```	

If you are using optional arguments:

``` matlab title="Function arguments with options" 
function force_N = getForce(mass_kg, accel_mps2,varargin)
% [Description]:
%	- Calculates resulting force using Second Newton's law.
% [Inputs]:
%	- mass_kg: Body Mass
%	- accel_mps2: Body acceleration
% [Optional arguments]:
%	- optionA: Description option A;
%	- optionB: Description option B;
% [Outputs]:
%	- force_N: resulting Force

Do your thing

end
```	

**Scalar or array**

* Prepare functions to receive both scalar or array**

``` matlab title="Example 1: Scalar or array" 
function force_N = getForce(mass_kg, accel_mps2)
	force_N = mass_kg .* accel_mps2;
end
```	
**Complexity**

Functions should have a single responsibility:

* E.g. A plotting function should plot (not convert multiple units, extract frequency response, etc.)

**Optional arguments**

* Use the same matlab convention if you need optinal arguments;

``` matlab title="Options" 
out = funcWithOptions(arg1,...
					  arg2,...
					  'option1',1.0,...
					  'option2',1.0,...
					  'option3',1.0)
```	

**Argument consistency**

* There should be only one way to provide an argument.

**The function interface is a contract:**

* Use the function description to define acceptable inputs;
* If the user disrespect the contract, an Error should not pass silently.

