# Concurrent Programming Intro

Computer Systems assignment A2. A thread-safe job queue in C, used to build multi-threaded versions of a simple `grep` and a bit-histogram tool.

## What's inside

- **`job_queue.c`** – a bounded, circular FIFO job queue protected by a mutex and two condition variables (producer/consumer). Worker threads block on an empty queue and exit when the queue is destroyed.
- **`fibs`** – small demo that computes Fibonacci numbers using the job queue.
- **`fauxgrep` / `fauxgrep-mt`** – searches files for a string and prints matching lines. The `-mt` version processes files in parallel.
- **`fhistogram` / `fhistogram-mt`** – counts how often each bit (0–7) is set across all bytes in the given files and prints a histogram. The `-mt` version processes files in parallel.

## Build

```sh
make
```

## Usage

```sh
./fauxgrep-mt [-n THREADS] NEEDLE PATH...
./fhistogram-mt [-n THREADS] PATH...
echo 10 | ./fibs
```

## Testing and benchmarking

```sh
bash testing.sh       # runs fibs, fauxgrep-mt and fhistogram-mt with 1–12 threads
bash benchmarking.sh  # times single- vs multi-threaded versions
```

## Results

fhistogram on 17 large data files, on a 6-core machine:

| Version | Time (s) | Throughput (MB/s) |
|---|---|---|
| Single-threaded | 4.21 | 11.5 |
| 1 thread | 3.38 | 14.4 |
| 2 threads | 2.17 | 22.4 |
| 4 threads | 1.27 | 38.1 |
| 6 threads | 1.02 | 47.9 |

The speedup is clearest on large inputs. With small files, the threading overhead outweighs the gain.
