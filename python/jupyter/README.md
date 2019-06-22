# Jupyter

[https://jupyterlab.readthedocs.io/en/stable/getting\_started/overview.html](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)

### 1.使用pip安装jupyter notebook

```bash
pip install jupyterlab
```

### 2.修改jupyter配置~/.jupyter/jupyter\_notebook\_config.json

```javascript
{
  "NotebookApp": {
    "ip": "*",
    "open_browser": false
  }
}
```

### 2.运行jupyter notebook

```bash
jupyter lab --port 8001 --no-browser
```

```python
from notebook.auth import passwd; passwd()
```

