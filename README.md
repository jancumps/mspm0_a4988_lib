# mspm0_a4988_lib
MSPM0 port of C++ driver for Allegro A4988 stepper motor.  
Example project: [stepper motor control with S-shaped ramps](https://community.element14.com/technologies/embedded/b/blog/posts/mspm0-project-with-vs-code-cmake-and-gcc---part-7c-stepper-motor-control-with-s-shaped-ramps)

How to use it in your project:

```cmake
include(FetchContent)

FetchContent_Declare(stepper_driver
  GIT_REPOSITORY "https://github.com/jancumps/stepper_driver_lib.git"
  GIT_TAG "origin/main"
)
FetchContent_MakeAvailable(stepper_driver)

FetchContent_Declare(mspm0_a4988
  GIT_REPOSITORY "https://github.com/jancumps/mspm0_a4988_lib.git"
  GIT_TAG "origin/main"
  # prevent makefile execution (if any)
  SOURCE_SUBDIR =
)
FetchContent_MakeAvailable(mspm0_a4988)

# ...


# list C++ module files here
set(SOURCES_MODULES
    ${stepper_driver_SOURCE_DIR}/source/stepper_driver.cpp
    ${mspm0_a4988_SOURCE_DIR}/source/a4988_mspm0.cpp
    # ....
```

```cxx
// import stepper_driver;              // wakeup class
import a4988_mspm0;                 // hw driver classes and registers

const uint32_t dir = GPIO_GRP_DRIVER_PIN_DIR_PIN;
const uint32_t n_enable = GPIO_GRP_DRIVER_PIN_N_ENABLE_PIN;
const uint32_t ms1 = GPIO_GRP_DRIVER_PIN_MS1_PIN;
const uint32_t ms2 = GPIO_GRP_DRIVER_PIN_MS2_PIN;
const uint32_t ms3 = GPIO_GRP_DRIVER_PIN_MS3_PIN;

const uint32_t microstep_x = 1;
const uint32_t max_step_freq = 300000;

// object to manage the a4988 IC used for motor1
using driver_t = a4988_mspm0::a4988_mspm0;
driver_t driver1(n_enable, ms1, ms2, ms3, GPIO_GRP_DRIVER_PORT);

int main() {
    SYSCFG_DL_init();
    
    driver1.init();
    driver1.microsteps(microstep_x);

    driver1.enable(true);

    // ....



```
