# w-worker
wombo combo worker
### python
```
import os
import multiprocessing
import subprocess

def run_instance(instance_id):
    env = os.environ.copy()
    env["NODE_ENV"] = "production"
    env["W_AI_API_KEY"] = "apikey"

    while True:
        try:
            print(f"[Instance {instance_id}] Starting...")
            subprocess.run(["wai", "run"], env=env, check=True)
        except subprocess.CalledProcessError as e:
            print(f"[Instance {instance_id}] Process exited with error: {e}. Restarting...")

if __name__ == "__main__":
    num_instances = 4  # Same as instances: 4 in PM2 config

    processes = []
    for i in range(num_instances):
        p = multiprocessing.Process(target=run_instance, args=(i,))
        p.start()
        processes.append(p)

    for p in processes:
        p.join()

```
Runing 
```
nohup python3 wai.py > output.log 2>&1 &

```
