# Coroutines
yield can also accept a value, this make it can communicate with other function. Only next() or send() has been called, the program will execute from one yield position to next.
```python
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)
```
In above case, producer produce message,then pass to consumer with yield. After consumer consume the message, then return to producer. During this progress, there is no lock to control queue and wait, it is efficient.

# Async IO 
Asynchronous IO makes it possible to access external resources without having to worry about slowing down or stalling your application.
## The async and await statements
The main concepts of asyncio are coroutines and event loops. A simple template of async and await usage like follows:
```python
import threading
import asyncio

async def hello():   #Function to do specific task
    # First part of task
    print('Hello world! (%s)' % threading.currentThread())

    # Second part of task, which is time-consuming
    await asyncio.sleep(1)  #This is the asynchronous version of time.sleep

    # Last part of task 
    print('Hello again! (%s)' % threading.currentThread())

loop = asyncio.get_event_loop()
tasks = [hello(), hello()]  #Define multi tasks
loop.run_until_complete(asyncio.wait(tasks))
loop.close()
```

## debug asynchronous functions - see where and how the functions were stalling
```python
>>> import asyncio
>>> async def stack_printer():
...
...
for task in asyncio.Task.all_tasks():
task.print_stack()
# Create an event loop
>>> loop = asyncio.get_event_loop()
# Create the task
>>> result = loop.run_until_complete(stack_printer())
```

## Event loop 
Two way to run the loop: loop.run_until_complete and loop.run_forever. If run_forever choosed, we need to add tasks to it. There are quite a few choices available within
the default event loops:
* call_soon : Add an item to the end of the (FIFO) queue so that the functions will be executed in the order in which they were inserted.
* call_soon_threadsafe : This is the same as call_soon except for being thread safe. The call_soon method is not thread safe because thread safety requires the usage of the global interpreter lock (GIL), which effectively makes your program single threaded at the moment of thread safety. 
* call_later : Call the function after the given number of seconds. 
* call_at : Call a function at a specific time related to the output of loop.time. Every integer after loop.time adds a second.
```py
>>> t = time.time()
>>> def printer(name):
... print('Started %s at %.1f' % (name, time.time() - t))
... time.sleep(0.2)
... print('Finished %s at %.1f' % (name, time.time() - t))
>>> loop = asyncio.get_event_loop()
>>> result = loop.call_at(loop.time() + .2, printer, 'call_at')
>>> result = loop.call_later(.1, printer, 'call_later')
>>> result = loop.call_soon(printer, 'call_soon')
>>> result = loop.call_soon_threadsafe(printer, 'call_soon_threadsafe')
>>> # Make sure we stop after a second
>>> result = loop.call_later(1, loop.stop)
>>> loop.run_forever()
Started call_soon at 0.0
Finished call_soon at 0.2
Started call_soon_threadsafe at 0.2
Finished call_soon_threadsafe at 0.4
Started call_later at 0.4
Finished call_later at 0.6
Started call_at at 0.6
Finished call_at at 0.8
```
## ansy processes
Execute external program asynchronously.
```py
>>> import time
>>> import asyncio
>>> t = time.time()
>>> async def async_process_sleeper():
...     print('Started sleep at %.1f' % (time.time() - t))
        # create a subprocess like subprocess.Popen
...     process = await asyncio.create_subprocess_exec('sleep', '0.1') 
...     await process.wait()
...     print('Finished sleep at %.1f' % (time.time() - t))
>>> loop = asyncio.get_event_loop()
>>> for i in range(5):
...     task = loop.create_task(async_process_sleeper())
>>> future = loop.call_later(.5, loop.stop)
>>> loop.run_forever()
Started sleep at 0.0
Started sleep at 0.0
Started sleep at 0.0
Started sleep at 0.0
Started sleep at 0.0
Finished sleep at 0.1
Finished sleep at 0.1
Finished sleep at 0.1
Finished sleep at 0.1
Finished sleep at 0.1
```
Another case with processes interactive input and output.
```py
async def run_script():
    process = await asyncio.create_subprocess_shell(
        'python3',
        stdout=asyncio.subprocess.PIPE,
        stdin=asyncio.subprocess.PIPE,
    )
    # Write a simple Python script to the interpreter
    process.stdin.write(b'\n'.join((
        b'import math',
        b'x = 2 ** 8',
        b'y = math.sqrt(x)',
        b'z = math.sqrt(y)',
        b'print("x: %d" % x)',
        b'print("y: %d" % y)',
        b'print("z: %d" % z)',
        b'for i in range(int(z)):',
        b'print("i: %d" % i)',
    )))
    # Make sure the stdin is flushed asynchronously
    await process.stdin.drain()
    # And send the end of file so the Python interpreter will
    # start processing the input. Without this the process will
    # stall forever.
    process.stdin.write_eof()
    # Fetch the lines from the stdout asynchronously
    async for out in process.stdout:
        # Decode the output from bytes and strip the whitespace
        # (newline) at the right
        print(out.decode('utf-8').rstrip())
    # Wait for the process to exit
    await process.wait()
if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.run_until_complete(run_script())
    loop.close()
```