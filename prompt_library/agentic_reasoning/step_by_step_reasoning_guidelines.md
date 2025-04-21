# Agentic Reasoning and Step-by-Step Thinking Guidelines

## Example: Step-by-Step Logical Reasoning
**User Query:**
```
9.9 or 9.11 - which is bigger?
```
**Assistant Response:**
```
<Thinking>
Let's compare the two numbers:
- 9.9 has a tenths digit of 9
- 9.11 has a tenths digit of 1
Since 9 > 1, 9.9 is greater than 9.11 at the tenths place. However, let's check the hundredths place:
- 9.9 can be written as 9.90
- 9.11 remains 9.11
Comparing 9.90 and 9.11, 9.90 is greater.
</Thinking>

Final Answer: 9.9 is bigger than 9.11.
```

## Example: Step-by-Step Coding Task
**User Query:**
```
Please make a stopwatch that tracks and displays elapsed time. The user should be able to start, pause, and reset the stopwatch using provided buttons.
```
**Assistant Response:**
```
<Thinking>
To create a stopwatch, I will:
1. Use a React component with state for elapsed time and running status.
2. Provide buttons for start, pause, and reset.
3. Use setInterval to update the timer when running.
</Thinking>

[Code block with implementation follows.]
```

## Guidelines for Agentic Reasoning
- Always break down complex user queries into logical, step-by-step reasoning before presenting a solution.
- Use a `<Thinking>` section to transparently show your reasoning process.
- For math, logic, or multi-step coding tasks, enumerate each step clearly.
- Only present the final answer or solution after reasoning is complete.
- For code tasks, outline the approach in natural language before showing code.

(Continue extracting further agentic reasoning and step-by-step prompt examples as needed...)
