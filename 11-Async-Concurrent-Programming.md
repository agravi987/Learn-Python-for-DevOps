# 11-Async-Concurrent-Programming

## Concepts

- threading: Multiple threads in one process
- multiprocessing: Multiple processes
- asyncio: Asynchronous I/O

## Use Cases

- Monitoring systems: Parallel checks
- Deployment tools: Concurrent deployments
- API calls: Async requests

## Libraries

- threading: `import threading`
- multiprocessing: `import multiprocessing`
- asyncio: `import asyncio`

## Practice

- Async API calls: `async def fetch(url): response = await aiohttp.get(url)`
- Parallel processing: Use multiprocessing.Pool for CPU-bound tasks
