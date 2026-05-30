# Safety Standards for Amateur Rocketry Software

## NASA Power of 10 Subset (Required)
**MUST** enforce these 3 rules:

### 1. **No Goto Statements**
   - **Why**: Prevents unpredictable control flow
   - **Violation**: `goto error_handler;`
   - **Fix**: Use structured error handling with return codes

### 2. **No Unbounded Recursion**
   - **Why**: Prevents stack overflow in embedded systems
   - **Violation**: Recursive function without base case or depth limit
   - **Fix**: Use iteration or add recursion depth counter

### 3. **Loops Must Have Static Bounds**
   - **Why**: Ensures predictable execution time
   - **Violation**: `while (sensor_reading() < threshold) { ... }`
   - **Fix**: `for (int i=0; i<MAX_ITER && sensor_reading()<threshold; i++) { ... }`

## MISRA C Essentials (Required)
**MUST** enforce these 3 rules:

### 4. **No Implicit Signedness Conversions**
   - **Why**: Prevents unexpected value changes
   - **Violation**: `int16_t x = -5; uint16_t y = x;`
   - **Fix**: Explicit cast: `uint16_t y = (uint16_t)x;`

### 5. **Initialize All Variables**
   - **Why**: Prevents use of uninitialized memory
   - **Violation**: `int x; y = x + 1;`
   - **Fix**: Initialize at declaration: `int x = 0;`

### 6. **Check Loop Counter Types**
   - **Why**: Prevents unexpected loop behavior
   - **Violation**: Using signed counter with unsigned comparison leading to infinite loops
   - **Fix**: Use matching signed/unsigned types for loop counters and bounds

## BARR-C Guidelines (Embedded Systems)
**MUST** enforce these 5 guidelines:

### 7. **Static globals prohibited**
   - **Why**: Prevents hidden coupling and initialization order issues
   - **Violation**: `static uint8_t buffer[256];` at file scope
   - **Fix**: Pass buffers explicitly or use dynamic allocation

### 8. **Enumerated types for state machines**
   - **Why**: Improves readability and prevents invalid state values
   - **Violation**: `#define STATE_BOOST 0` and checking `state == STATE_BOOST`
   - **Fix**: `typedef enum { STATE_PRE_FLIGHT, STATE_BOOST, STATE_COAST, ... } flight_state_t;`
   - **Note**: This guideline applies specifically to state machine implementations. For pure calculation functions (like sensor conversions), enum types are not required.

### 9. **Constants for magic numbers**
   - **Why**: Improves maintainability and documents intent
   - **Violation**: `if (pressure > 101325) { ... }`
   - **Fix**: `#define SEA_LEVEL_PRESSURE 101325U` then `if (pressure > SEA_LEVEL_PRESSURE) { ... }`
   - **Common examples in rocketry**:
     - Sea level pressure: `SEA_LEVEL_PRESSURE_PA = 101325U`
     - Temperature lapse rate: `TEMP_LAPSE_RATE_KPM = 0.0065F`
     - Universal gas constant: `UNIVERSAL_GAS_CONSTANT = 8.314462618F`
     - Gravitational acceleration: `GRAVITY_MS2 = 9.80665F`

### 10. **Return status from all functions**
    - **Why**: Enables error detection and propagation
    - **Violation**: `float calculate_altitude(uint32_t pressure_pa, float temperature_c);`
    - **Fix**: Either:
       - Return status code: `int calculate_altitude(uint32_t pressure_pa, float temperature_c, float* altitude_out);`
       - Or return struct: `altitude_result_t calculate_altitude(uint32_t pressure_pa, float temperature_c);`
       - Where `altitude_result_t` contains `{float altitude; int status;}`

### 11. **Compiler warnings treated as errors**
    - **Why**: Catches potential issues early in development
    - **Fix**: Configure build system to treat warnings as errors (e.g., `-Werror` for GCC/Clang)
