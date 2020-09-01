In general, for CPU-bound processing, using threading can easily lead to worse
performance than not using concurrency at all. One solution to this is to write
the code in Cython.

For I/O-bound processing (e.g., networking), using concurrency can produce
dramatic speedups. In these cases, network latency is often such a dominant
factor that whether the concurrency is done using threading or multiprocessing
may not matter.