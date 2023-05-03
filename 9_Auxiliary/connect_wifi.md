## Connect PYNQ-ZU to WiFi

On PYNQ-ZU, run these commands:
```
sudo -s
source /etc/profile.d/pynq_venv.sh
python3
```

In Python prompt, type:
```python
from pynq.lib import Wifi
port = Wifi()
port.connect('dlink-2.4GHz', '12345678', force=True)
```

If you fail connecting to WiFi, use:
```
port.reset()
```

Or try to reboot PYNQ-ZU.