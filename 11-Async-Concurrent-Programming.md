# 11-Async-Concurrent-Programming ⚡

Async and concurrent programming help Python do more work in less time. In DevOps, this is useful when checking many servers, calling many APIs, running many health checks, or processing many files.

## 🎯 What You Will Learn

- The difference between concurrency and parallelism.
- When to use `threading`, `multiprocessing`, and `asyncio`.
- How async API calls work.
- How to run multiple tasks at once.
- How to build a mini concurrent URL checker.

## 🧠 Concurrency vs Parallelism

Concurrency means handling multiple tasks during the same time period. A program may switch between tasks while waiting.

Parallelism means truly running multiple tasks at the same time, usually on multiple CPU cores.

Simple rule:

- Use `asyncio` for many waiting tasks, like API calls.
- Use `threading` for blocking I/O tasks when libraries are not async.
- Use `multiprocessing` for CPU-heavy work.

## 🧵 threading

Threads run multiple tasks in one process. They are useful for I/O-bound work like checking files, waiting on network calls, or running commands.

```python
import threading
import time

def check_service(name):
    time.sleep(2)
    print(f"✅ {name} checked")

threads = []

for service in ["api", "db", "cache"]:
    thread = threading.Thread(target=check_service, args=(service,))
    thread.start()
    threads.append(thread)

for thread in threads:
    thread.join()
```

Threads are not ideal for CPU-heavy tasks because of Python's Global Interpreter Lock, often called the GIL.

## 🧠 multiprocessing

Multiprocessing starts separate Python processes. This can use multiple CPU cores.

```python
from multiprocessing import Pool

def square(number):
    return number * number

with Pool() as pool:
    results = pool.map(square, [1, 2, 3, 4, 5])

print(results)
```

Use multiprocessing for CPU-heavy tasks like compression, image processing, parsing large files, or heavy calculations.

## 🌊 asyncio

`asyncio` is Python's built-in async framework. It is excellent for many network calls.

```python
import asyncio

async def check_service(name):
    await asyncio.sleep(2)
    print(f"✅ {name} checked")

async def main():
    await asyncio.gather(
        check_service("api"),
        check_service("db"),
        check_service("cache")
    )

asyncio.run(main())
```

`await` means: pause this task until the result is ready, and let other tasks run meanwhile.

## 🌐 Async HTTP with aiohttp

Install:

```bash
pip install aiohttp
```

Example:

```python
import asyncio
import aiohttp

async def fetch_status(session, url):
    async with session.get(url) as response:
        return url, response.status

async def main():
    urls = [
        "https://api.github.com",
        "https://www.python.org",
        "https://pypi.org"
    ]

    async with aiohttp.ClientSession() as session:
        tasks = [fetch_status(session, url) for url in urls]
        results = await asyncio.gather(*tasks)

    for url, status in results:
        print(url, status)

asyncio.run(main())
```

This checks multiple URLs without waiting for each one sequentially.

## 📌 Choosing the Right Tool

Use this guide:

- Many API calls: `asyncio` with `aiohttp`.
- Many blocking operations: `threading`.
- CPU-heavy calculations: `multiprocessing`.
- Simple script with only a few tasks: normal sequential code may be enough.

## ⚠️ Common Mistakes

- Using async for CPU-heavy tasks.
- Forgetting `await`.
- Calling blocking functions inside async code.
- Creating too many concurrent requests and hitting rate limits.
- Ignoring error handling in gathered tasks.

## 🛠️ Mini Project: Concurrent URL Health Checker

Goal: Build a script that checks multiple URLs concurrently and creates a report.

### Step 1: Install aiohttp

```bash
pip install aiohttp
```

### Step 2: Create a URL List

```python
urls = [
    "https://api.github.com",
    "https://www.python.org",
    "https://pypi.org"
]
```

### Step 3: Write an Async Fetch Function

```python
import aiohttp

async def check_url(session, url):
    try:
        async with session.get(url, timeout=10) as response:
            return {
                "url": url,
                "status": response.status,
                "healthy": response.status < 400
            }
    except Exception as error:
        return {
            "url": url,
            "status": "error",
            "healthy": False,
            "error": str(error)
        }
```

### Step 4: Run Checks Concurrently

```python
import asyncio
import aiohttp

async def main():
    async with aiohttp.ClientSession() as session:
        tasks = [check_url(session, url) for url in urls]
        return await asyncio.gather(*tasks)

results = asyncio.run(main())
```

### Step 5: Write the Report

```python
from pathlib import Path

lines = ["URL Health Report", "================="]

for result in results:
    icon = "✅" if result["healthy"] else "🚨"
    lines.append(f"{icon} {result['url']} - {result['status']}")

Path("url-health-report.txt").write_text("\n".join(lines))
```

### Step 6: Improve It

- Read URLs from `urls.txt`.
- Add response time measurement.
- Limit concurrency with `asyncio.Semaphore`.
- Exit with code `1` if any URL fails.
- Send failed URLs to a notification channel.

## ✅ Quick Revision

- Concurrency helps with waiting-heavy tasks.
- Parallelism helps with CPU-heavy tasks.
- `threading` works for blocking I/O.
- `multiprocessing` uses multiple CPU cores.
- `asyncio` is great for many network requests.
