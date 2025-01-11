# Project Threads

## Overview

This project is an implementation of thread management and scheduling mechanisms in an operating system context, focusing on functionality and synchronization. The project comprises three main phases:

1. **Alarm Clock** - Efficient thread sleep and wakeup implementation.
2. **Priority Scheduling** - Handling priority donation and managing thread scheduling based on priorities.
3. **Advanced Scheduler** - Implementation of a Multi-Level Feedback Queue Scheduler (MLFQS).

## Team Members
- **Ashish Ramagoni**: [ashishgo@buffalo.edu](mailto:ashishgo@buffalo.edu)
- **Wendan Zhao**: [wendanzh@gmail.com](mailto:wendanzh@gmail.com)
- **Yashwanth Chennu**: [ychennu@buffalo.edu](mailto:ychennu@buffalo.edu)

## Features

### Alarm Clock
Implements a sleep mechanism for threads:
- Threads calculate a wake-up time and self-block until this time is reached.
- Timer interrupt checks if any thread's wake-up time has arrived and unblocks it.
- Efficient use of CPU resources by avoiding busy waiting.

### Priority Scheduling
Implements thread scheduling based on priority:
- Priority donation to avoid priority inversion.
- Nested priority donation propagation.
- Ensures the highest-priority thread always executes first.

### Advanced Scheduler
Introduces a fair scheduling policy:
- Multi-Level Feedback Queue Scheduler (MLFQS).
- Dynamic thread priority adjustment based on CPU usage and niceness values.
- Implements fixed-point arithmetic for priority and load-average calculations.

## Project Structure

```
root
├── src/              # Source code for the project
│   ├── threads/      # Thread implementation and modifications
│   └── tests/        # Test cases for each phase
└── README.md         # Project documentation
```

### Key Files
- `src/threads/thread.c`: Core thread management logic.
- `src/threads/timer.c`: Timer and alarm clock implementation.
- `src/threads/synch.c`: Synchronization primitives and priority donation.

## Design Highlights

### Alarm Clock
- **Data Structures**: Added fields to `struct thread`:
  - `bool waiting_status`: Tracks if the thread is waiting for a timer event.
  - `int64_t time_to_wake`: The tick count when the thread should wake up.
  - `struct semaphore s`: Used to block and unblock threads.
- **Key Functions**:
  - `timer_sleep()`: Calculates wake-up time and blocks the thread.
  - `timer_interrupt()`: Wakes up threads when their wait time is over.

### Priority Scheduling
- **Data Structures**:
  - `int base_priority`: Tracks the original priority of a thread.
  - `struct list locks_holding`: Tracks locks held by a thread.
  - `struct thread *lock_holder`: Tracks the lock that a thread is waiting on.
- **Algorithms**:
  - Implements direct and nested priority donation.
  - Utilizes efficient list operations to identify and wake the highest-priority thread.

### Advanced Scheduler
- **Data Structures**:
  - `int nice`: Niceness of the thread.
  - `fixed_point_t recent_cpu`: Tracks recent CPU usage.
  - `fixed_point_t load_avg`: System-wide average number of ready threads.
- **Key Calculations**:
  - Dynamic priority adjustment every tick.
  - Load average and recent CPU recalculations.

## Testing

### Test Cases
The project includes comprehensive test cases:
- **Alarm Clock**: Single-thread and multi-thread sleep-wake tests.
- **Priority Scheduling**: Priority donation, nested donation, and semaphore tests.
- **Advanced Scheduler**: Fairness tests for threads with varying niceness and CPU usage.

### Running Tests
1. Navigate to the `src/threads` directory.
2. Build the project: `make`
3. Run tests: `make check`

## Usage
- Build the project with `make`.
- Run the Pintos kernel with `pintos --run "<test_case_name>"`.
- Analyze output logs for test results.

## Challenges and Improvements
### Challenges
- Implementing efficient wake-up mechanisms in the timer interrupt handler.
- Managing priority donation with nested dependencies.

### Potential Improvements
- Use data structures like priority queues for improved scheduling efficiency.
- Separate ready lists into multiple queues for different priority levels to reduce traversal overhead.
