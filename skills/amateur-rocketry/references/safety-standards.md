# Safety Standards for Amateur Rocketry Software

## NASA Power of 10 Subset (Required)
**MUST** enforce these 3 rules:

1. **No Goto Statements**
   - **Why**: Prevents unpredictable control flow
   - **Violation**: `goto error_handler;`
   - **Fix**: Use structured error handling with return codes

2. **No Unbounded Recursion**
   - **Why**: Prevents stack overflow in embedded systems
   - **Violation**: Recursive function without base case or depth limit
   - **Fix**: Use iteration or add recursion depth counter

3. **Loops Must Have Static Bounds**
   - **Why**: Ensures predictable execution time
   - **Violation**: `while (sensor_reading() < threshold) { ... }`
   - **Fix**: `for (int i=0; i<MAX_ITER && sensor_reading()<threshold; i++) { ... }`

## MISRA C Essentials (Required)
**MUST** enforce these 3 rules:

4. **No Implicit Signedness Conversions**
   - **Why**: Prevents unexpected value changes
   - **Violation**: `int16_t x = -5; uint16_t y = x;`
   - **Fix**: Explicit cast: `uint16_t y = (uint16_t)x;`

5. **Initialize All Variables**
   - **Why**: Prevents use of uninitialized memory
   - **Violation**: `int value; return value;`
   - **Fix**: `int value = 0; return value;`

6. **Limit Macro Usage**
   - **Why**: Reduces preprocessor complexity
   - **Violation**: `#define MAX(a,b) ((a)>(b)?(a):(b))`
   - **Fix**: Use static inline functions

## BARR-C Guidelines (Recommended)
**SHOULD** follow these 2 guidelines:

7. **Minimize Global Variables**
   - **Why**: Reduces coupling and unintended side effects
   - **Violation**: Non-static global variables
   - **Fix**: Use `static` for file-scoped variables or pass as parameters

8. **Check All Function Return Codes**
   - **Why**: Ensures error handling
   - **Violation**: `init_sensor();` // ignoring return
   - **Fix**: `if (init_sensor() != SUCCESS) handle_error();`

## Implementation Guidance
- Apply rules during code review using checklists
- Start with NASA Power of 10 as baseline
- Use compiler warnings (+Wall -Wextra -pedantic)
- Consider lightweight tools: CPPcheck, Clang-Tidy
- Document exceptions with justification when unavoidable

