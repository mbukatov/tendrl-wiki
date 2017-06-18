**Pre-requisites for enabling Tendrl profiler on a storage node:**
- `yum install python-devel python-pip`
- `yum group install "Development Tools"`
- `pip install GreenletProfiler`

**About Tendrl profiler**:
- Tendrl comes with an internal per service profiler which collects function call statistics (example below)
- Enable profiling for a service by adding `with_internal_profiler: True` to the service's local config file.


**Eg: to enable profiling for tendrl-node-agent**
- edit `/etc/tendrl/node-agent/node-agent.conf.yaml` and add flag `with_internal_profiler: True`
- restart tendrl-node-agent service `systemctl restart tendrl-node-agent`
- To gather profiling stats, stop the service after some amount of time `systemctl stop tendrl-node-agent`
- Function Profiling data in formats (pstat, callgrind, ystat) will be saved at `/var/lib/tendrl/profiling/node_agent/last_run_func_stat.pstat`
- Output https://gist.github.com/r0h4n/b8905c83789d40b434414957019d247d


Note: profiling data can be visualized via https://github.com/ymichael/cprofilev (for *.pstat) and via http://valgrind.org/docs/manual/cl-manual.html (for *.callgrind)
