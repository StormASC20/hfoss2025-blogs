---
layout: post
categories: 
- Contribution
- Embedded
- SHED Makerspace

author: Aaron Chan
---

# Project Introduction

For my contribution, I decided to help the RIT SHED Makerspace with their access control firmware. I decided to do this because I am a frequent user of the RIT SHED facilities as a member of RIT Launch Initiative. I saw an opportunity to help improve the custom devices the SHED uses to ensure only authorized and trained users operate the SHED's equipment.

The project is currently maintained mainly by SHED employees, but is open-sourced under a Creative Commons license. However, since it is lesser known that some of SHED's software (and hardware) is open-sourced, there is little outside attention given to it. The reason I knew about it was because I the person that mostly maintains these projects, Jim Heaney who mainly develops open-source hardware. Therefore, I thought it would also be a good opportunity to work with him towards improving the software since he mostly comes from a hardware background. After getting approval from my TA Adrian, I decided to start contributing with a massive overhaul to the access control firmware.

The project itself has no issue tracking and little documentation, so most students that have taken HFOSS will most likely not want to contribute to this project if they only gave it a quick glance. However, the ease of accessibility to other contributors and simple codebase should not be discounted. If I was to give a CommArch presentation on this community though, I would still recommend it regardless of the aformentioned issues. I think this project and similar SHED projects are still in its infancy when it comes to open-source, but because the projects are simple enough and you can easily work with most RIT affiliated individuals, these projects can easily be onboarded onto. I hope this blog post might motivate other individuals to participate as well in the future.

![Access control device not accepting my ID card](/hfoss2025-blogs/assets/images/aarc10/access_control_not_working.jpg)
*One of the access control devices wasn't accepting me and my friend's ID cards despite having the approved trainings. Turns out the reader for that station in the Textiles and Electronics room in the SHED was faulting out throughout the day.*

## Finding Issues
SHED software projects are relatively informal and there is no ticket tracking system. Instead I looked throughout the codebase for issues I could find. Since Jim mainly specializes in hardware, there were several potential issues I was able to identify quickly before starting. The following include

- Race Conditions - Shared resources used across tasks without locking.
- Delay Drift - Sampling something over I/O and delaying for a set period of time with no regard to the time processing the results of an I/O operation.
```
while(1) {
    result = 10_HZ_IO_CALL()
    PROCESS_DATA(result)
    delay(100);
}
```
- Ungraceful Error Handling - Resetting the chip after a single failure.
```
if (resp < 0) {
  ESP.restart();
}
```

- Buffer Overflows
```
uint8_t uid[10] = {0};
...
uid[10] = {0};
```

## Fixes
Thankfully, the file structure was organized enough to modify each file one a time for the most part. I went through each file individually, making changes as I saw fit. 

- For solving the race conditions. I implemented a new class that implements a Mutex. I took advantage of using C++ templates for future developers to easily create new shared variables without having to create a new semaphore for every shared variable they create, and make the proper calls for it throughout the code. Also, since most of the shared variables are currently integers and booleans, I also added operator overloading, so users can use these variables as if they were primitive types, such as add and assign, and incrementing operations.
```
template <typename T>
class Atomic {
public:
  Atomic(T initialValue = T()) : value(initialValue) {
    mutex = xSemaphoreCreateMutex();
    if (mutex == NULL) {
      // Should never occur in practice
      USBSerial.println("Failed to create mutex for Atomic. Halting");
      while(1);
    }
  }

  T get() {
    T result;
    if (xSemaphoreTake(mutex, portMAX_DELAY) == pdTRUE) {
      result = value;
      xSemaphoreGive(mutex);
    }
    return result;
  }

  template <typename U = T>
  typename std::enable_if<std::is_arithmetic<U>::value, Atomic&>::type
  operator+=(U rhs) {
    if (xSemaphoreTake(mutex, portMAX_DELAY) == pdTRUE) {
      value += rhs;
      xSemaphoreGive(mutex);
    }
    return *this;
  }
  ...
```

- To fix delay drifts, I leveraged the usage of FreeRTOS further, by storing the last wakeup time and how long we want the delay for. vTaskDelayUntil will check the last time the task was active, sleep the desired amount of time after it, instead of a consistent time no matter what, and then update the wakeup time variable. So, if we want some operation done every 50 milliseconds, it will track the last time the operation was done, do whatever extra processing happens afterwards and only sleep 50 milliseconds minus the extra processing time.
```
  const TickType_t xFrequency = 50 / portTICK_PERIOD_MS;
  xLastWakeTime = xTaskGetTickCount();
  while(1){
    ...
    vTaskDelayUntil(&xLastWakeTime, xFrequency);
```

- I made error handling more graceful in several ways. If an HTTP response fails, try again after waiting a few seconds, instead of restarting the entire chip. Also, reading from USBSerial returns any potential errors, which was ignored in the original code.
- There was the intention of zeroing out a buffer in one part of the codebase, but it instead accessed an array outside of its bounds and setting it to 0 instead. I was told by Jim that the original was part of an example he copied. So,

```
uid[10] = {0};
```

should instead be

```
memset(uid, 0, sizeof(uid));
```

## Blockers
Unfortunately, due to lack of access to hardware I have been unable to personally test my changes on the hardware itself. However, part of the purpose of the assignment was to interact with a community and since this project doesn't really have many external contributors, I was able to work closely with Jim in person. This also allowed me to teach Jim new software techniques to incorporate into his future work as well. Testing and deployment has been blocked for a short time period at the time of writing since Jim is now on a business trip. When he returns, we will continue incorporating these changes into production which will then see usage by all members of the RIT community that uses SHED resources.

Since SHED software projects are somewhat informal, there is little documentation on setting up for development and navigating the codebase too. However, the codebase wasn't too big for this to be an issue and Jim was accessible through Slack and in person which helped resolve any questions or issues I have had.

![Working with Jim in T&E](/hfoss2025-blogs/assets/images/aarc10/jim_t_and_e.jpg)
*Jim and me debugging hardware in the Textiles and Electronics room.*

## Conclusion
Overall, it was a fun project to work on. I had the opportunity to work closely with a primary contributor of an open-source project and I know my contributions will directly impact the RIT community overall. I also hope to raise awareness that there are small open-source RIT projects that could benefit from even small contributions and they can be easier to work with in some cases compared to working with larger open-source projects which may have harder accessibility to primary contributors of larger projects and more bureacracy for getting changes through. I could potentially help Jim Heaney and the SHED further in the future, by helping them set up contributor guidelines and documentation to make contributing more accessible to other people. 
