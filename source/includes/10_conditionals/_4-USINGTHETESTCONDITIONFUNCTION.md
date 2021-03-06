## Using the Test Condition Function

### Signature

`TestCondition (name, tParams)`

In order to evaluate the expression, the driver must implement the TestCondition function in their lua code.

| Parameter | Description |
| --- | --- |
| str | name: The name of the condition being tested. This is the name found in the \<name\> defined in the conditional xml. |
| table | tParam: Table of parameters. This varies depending on the Conditional Type  `<type>` tag as defined in the conditional xml. |

Conditional Types:

- SIMPLE: empty table
- BOOL: empty table
- NUMBER: table of two name/value pairs:
- tParams “VALUE” contains the INTEGER macro value of the code item in programming.
- tParams “LOGIC” contains the LOGIC macro value of the code item in programming. Possible LOGIC values are:
EQUAL, 
NOT\_EQUAL, 
LESS\_THAN, 
LESS\_THAN\_OR\_EQUAL, 
GREATER\_THAN, 
GREATER\_THAN\_OR\_EQUAL.

You may use `C4:EvaluateExpression` to simplify the evaluation of the LOGIC field.


STRING: table of two name/value pairs:
`tParams[“VALUE”]` contains the STRING macro value of the code item in programming.
`tParams[“LOGIC”]` contains the LOGIC macro value of the code item in programming. Possible LOGIC values are EQUAL and NOT\_EQUAL.

You may use `C4:EvaluateExpression` to simplify the evaluation of the LOGIC field.



LIST: table of two name/value pairs:
`tParams[“VALUE”]` contains the STRING macro value of the code item in programming.
`tParams[“LOGIC”]` contains the LOGIC macro value of the code item in programming. Possible LOGIC values are EQUALand NOT\_EQUAL.\_

You may use `C4:EvaluateExpression`n to simplify the evaluation of the LOGIC field.



ROOM: table of two name/value pairs
`tParams[“VALUE”]` contains the room id of the ROOM macro value of the code item in programming.
`tParams[“LOGIC”]` contains the LOGIC macro value of the code item in programming. Possible LOGIC values are EQUALand NOT\_EQUAL.

You may use `C4:EvaluateExpression` to simplify the evaluation of the LOGIC field.



DEVICE: table of two name/value pairs
`tParams[“VALUE”]` contains the device id of the DEVICE macro value of the code item in programming.
`tParams[“LOGIC”]` contains the LOGIC macro value of the code item in programming. Possible LOGIC values are EQUALand NOT\_EQUAL.

You may use `C4:EvaluateExpression` to simplify the evaluation of the LOGIC field.


### Returns
The function must return a Boolean value. If the expression results to true then return true, otherwise return false.

```lua
Sample TestCondition function


function TestCondition(name, tParams)
	local retVal = false

	if (name == "SIMPLE_LIGHT_ON") then
		retVal = (gLightLevel > 0)
	elseif (name == "SIMPLE_LIGHT_OFF") then
		retVal = (gLightLevel == 0)
	elseif (name == "BOOL_LIGHT_ON") then
		local logic = tParams["LOGIC"]
		local value = tParams["VALUE"]
		retVal = (value == "On")
	elseif (name == "BOOL_LIGHT") then
		local value = tParams["VALUE"]
		if (value == "On") then
			retVal = gLightLevel > 0
		elseif (value == "Off") then
			retVal = gLightLevel == 0
		end
	elseif (name == "NUMBER_LIGHT_LEVEL") then
		local logic = tParams["LOGIC"]
		local value = tonumber(tParams["VALUE"])
		retVal = C4:EvaluateExpression(logic, gLightLevel, value)
	elseif (name == "STRING_LIGHT") then
		local logic = tParams["LOGIC"]
		local value = tonumber(tParams["VALUE"])
		retVal = C4:EvaluateExpression(logic, gLightLevel, value)
	elseif (name == "LIST_LIGHT_LEVEL") then
		local logic = tParams["LOGIC"]
		local strValue = tParams["VALUE"]
		local value = tonumber(string.match(strValue,"(%d.+).+"))
		LogTrace("value = " .. value)
		retVal = C4:EvaluateExpression(logic, gLightLevel, value)
	elseif (name == "ROOM_SELECTION") then
		local logic = tParams["LOGIC"]
		local strValue = tParams["VALUE"]
		retVal = C4:EvaluateExpression(logic, someRoomId, value)
	elseif (name == "DEVICE_SELECTION") then
		local logic = tParams["LOGIC"]
		local strValue = tParams["VALUE"]
		retVal = C4:EvaluateExpression(logic, someDeviceId, value)
	end

	LogTrace("c4test: Result = " .. tostring(retVal))
	return retVal
end
```

