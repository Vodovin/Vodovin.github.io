---
title: 'Minikernel - Operating System Extension'
description: C implementation of kernel modifications to introduce multiprogramming, mutex-based synchronization, and Round-Robin scheduling in an educational operating system.
publishDate: 'Oct 2 2023'
isFeatured: true
seo:
  image:
    src: '../../assets/images/project-2-0.jpg'
    alt: Project preview
---

![Project preview](../../assets/images/project-2-0.jpg)

**Project Overview:**
This project consists of modifying an initial version of a minikernel to include new functionalities and add multiprogramming capabilities.

## Design Objectives

The main objective of the architecture modifications was to interact with an initial version of the minikernel that already included initialization, exception handling, and a basic system call infrastructure. The design changes focused on three core areas:
*   **Interrupt Handling:** Modifying and completing the code related to external and software interrupt handling.
*   **System Calls:** Adding new system calls by modifying kernel files (`kernel.c`, `kernel.h`, `llamsis.h`) and user libraries (`serv.c`, `servicios.h`) to implement the requested features.
*   **Data Structures:** Adapting existing structures like the Process Control Block (BCP), the process table, and the ready queue to support the new Round-Robin scheduling algorithm and process blocking.

## Main Features

1. **Process Blocking and Multiprogramming (Sleep):**
   * Implementation of the `dormir` system call, allowing a process to voluntarily block itself for a specific period of time, giving the system multiprogramming characteristics.
   * The Process Control Block (BCP) was modified to include a `tiempo_dormir` variable.
   * A new list called `lista_dormidos` was created to store blocked processes.
   * The clock interrupt routine was updated to decrease the sleep time of blocked processes and transition them back to the ready state once their time reaches zero.

2. **Mutex-based Synchronization:**
   * Implementation of mutual exclusion for processes, supporting both recursive and non-recursive mutex types.
   * A new `mutex_t` structure was defined, and the BCP was modified to include arrays of mutex descriptors.
   * The system features a full suite of calls: `crear_mutex`, `abrir_mutex`, `lock`, `unlock`, and `cerrar_mutex`.
   * Advanced blocking is handled when the system limit of available mutexes is reached, waking up blocked processes once a mutex is closed and freed.
   * The kernel ensures the implicit release of all mutexes held by a process when it terminates.

3. **Round-Robin Scheduling:**
   * Replacement of the default FIFO scheduling algorithm with a Round-Robin approach.
   * Time slices are defined by the `TICKS_POR_RODAJA` constant, introducing a `ticks_rodaja` variable to the BCP.
   * To prevent synchronization issues, the kernel acts as non-preemptive; context switches are not allowed while a process is executing device handler routines or system calls.
   * Context switching is managed via a non-preemptive software interrupt that moves the current process to the end of the ready queue once its time slice is consumed.

## Technology Stack

- **Programming Language:** C for low-level kernel development, data structure manipulation, and pointer arithmetic.
- **Environment & OS:** Custom Minikernel architecture, handling direct hardware clock interrupts, software interrupts, and system call dispatching.
- **Concurrency & Synchronization:** Development of custom synchronization primitives (Mutex), process state management, and CPU scheduling algorithms (Round-Robin).

## Test Cases

The code was subjected to various tests via the `init.c` file to ensure its stability:
* **Sleep functionality (`prueba_dormir`):** Tests two processes sleeping for 1 and 3 seconds respectively, verifying that they sleep for the correct number of ticks and wake up concurrently as expected.
* **Mutex Validation (`prueba_mutex1` & `prueba_mutex2`):** Tests the creation, opening, and closing of mutexes, verifying system and process limits, and blocking processes when no system space is available. It also evaluates non-recursive vs. recursive locking, multiple lock/unlock sequences, and the correct implicit closure of mutexes upon process termination.
* **Round-Robin Execution (`prueba_RR2`):** Verifies the scheduler by running 5 processes simultaneously alongside a "mudo" program that consumes CPU through arithmetic calculations without writing to the screen.