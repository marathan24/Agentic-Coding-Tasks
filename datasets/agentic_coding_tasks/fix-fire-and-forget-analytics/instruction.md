
The current response stream lags because it waits for internal processing to finish.
Decouple these operations by moving the secondary logic to background tasks.
Implement an asynchronous queue to handle data without blocking the main flow.
The system must yield content immediately while processing happens later.
Ensure user-facing output is prioritized over internal bookkeeping.
Refactor the generation loop to guarantee smooth delivery.