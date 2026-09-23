# Monumo Job Queue Demonstrator

A small demonstration job queue system, built to explore the kind of infrastructure needed to safely expose a compute heavy engine, like Monumo's Anser, to external customers.

## Why this exists

Opening up a powerful internal engine, like Monumo's Anser, to external customers is a genuinely hard infrastructure problem, queuing, scheduling, retrying and isolating jobs safely, so the system stays reliable even when things fail. This project is my own attempt to understand and demonstrate that problem directly, rather than just reading about it.

## What it does

This is a FastAPI application that accepts jobs, processes them in the background, and handles failure realistically rather than assuming everything succeeds first time.

-Job submission and status checking.** Jobs are submitted through a simple API, then run in a background thread, so the caller is not left waiting.
-Retry logic.** Each job has a chance of failing, simulating a real, unreliable process. Failed jobs are automatically retried, up to five attempts, before being marked as genuinely failed.
-Containerisation.** The whole application is packaged with Docker, so it runs identically anywhere, not just on my own machine.
-Versioned results storage.** Every finished job, successful or failed, is saved as its own file, so results survive a restart rather than only living in memory.

## Running it

Build the image.

docker build -t monumo-job-queue .


Run it, linking the results folder so saved job files appear on your own machine too.

docker run -p 8000:8000 -v "${PWD}/results:/app/results" monumo-job-queue


Then visit `http://127.0.0.1:8000/docs` to submit jobs and check their status interactively.

## What this is not

This is a small, honest demonstration, not a production system. It does not yet include things like authentication, a proper database, or handling genuinely concurrent load at scale. It was built to show real understanding of the problem, not to claim it is finished.