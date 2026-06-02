# Exercise 4: Debugging and Evaluating Application Logic

### Estimated Duration: 30 minutes

## Lab Scenario

In this exercise, you will debug and evaluate the core application logic. You will learn how to run the orchestrator, activate tracing for debugging purposes, and assess the generated content's quality. Additionally, you will explore how the application evaluates key response metrics such as Coherence, Fluency, Relevance, and Groundedness.

## Lab Objectives

After you complete this exercise, you will be able to:

 - Debug the application by running the orchestrator logic and enabling tracing.

### Task 1: Debug the application by running the orchestrator logic and enabling tracing.

In this task, you will run the orchestrator logic to simulate the application flow and enable tracing for debugging purposes. By doing so, you'll track the execution of Python functions and capture detailed traces, which will help in diagnosing any issues within the application's logic.

1. Navigate back to your **Git Bash** terminal, and open a new **Git Bash** terminal and run the following command to go back to `/src/api` directory.

   ```bash
   cd src/api
   ```

   >Make sure you are running this in a new **Git Bash** terminal tab, because the application should be running.

1. Run the below command to enable `LOCAL_TRACING`.

    ```bash
   export LOCAL_TRACING=true
   ```
   >**Lab Tip:** Tracing is the process of tracking and logging the execution flow of an application. Enabling tracing helps capture detailed insights into the operations, allowing you to monitor the sequence of function calls, identify performance bottlenecks, and troubleshoot issues by providing a step-by-step view of the system's behavior. This is particularly useful for debugging and improving the application.
   
1. You will now run the orchestrator with local tracing enabled, which will provide more detailed logs for better visibility into the process.

   ```bash
   python -m orchestrator
   ```

   >**Note:** You can ignore the warning message that appears when running the above command. Detailed logs will still be available for review.
  
   ![](../media/ex3img2.png)

## Summary

In this exercise, the orchestrator logic was run with tracing enabled to debug the article generation process.

### You have successfully completed the Lab!!
