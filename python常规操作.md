```
# qt #
# 防止gui卡顿, 界面上需要采用无阻塞代码

QtCore import QTimer, QThread, QProcess 不阻塞操作事件

QProcess.startDetached(指令), 分离式运行指令, 如dir/python *.py, killall -9 **等

QTimer.stop() / QTimer.start(毫秒) 停止/每隔几秒触发,  !!!gui关闭定时器就关闭!!!


# 设计界面 和 转py
[系统]
sudo apt install build-essential pyqt5-dev-tools qttools5-dev-tools qttools5-dev
[python]
pip install pyqt5 pyqt5-tools

[pycharm extenal tools]
[设计]
Program:/bin/designer
Arguments:$FileDir$/$FileName$
WorkDir:$FileDir$

[转py]
Program:/bin/pyuic5
Arguments:$FileName$ -o $FileNameWithoutExtension$.py
WorkDir:$FileDir$

# 线程池 #
from concurrent.futures import ThreadPoolExecutor
# 多线程操作
with ThreadPoolExecutor(max_workers=1) as t:
  t.submit(函数名, 参数)

# 开进程运行指令
out = subprocess.run(指令, shell=True,
                     stderr=subprocess.STDOUT)

# request #
# 文件上传
zipTmp = open(dir + '.zip', 'rb')
requests.post(url=img_url,
             files={key: (None, val),
                    "zipFile": ('*.zip', zipTmp)},
             timeout=(3.5, 60),
             headers={'Connection': 'close', key: val})
if zipTmp is not None and dir is not None:
    try:
        zipTmp.close()
        os.remove(dir + '.zip')
    except Exception as e:
        logging.error(e)
        raise e

# post
requests.post(url=token_url,
             json={key: val},
             timeout=(3.5, 60),
             headers={'Connection': 'close'})

# 定时任务 #
# 初始化 sched 模块的 scheduler 类
# sched 模块不是循环的，一次调度被执行后就 Over 了，如果想再执行，请再次 enter
scheduler = sched.scheduler(time.time, time.sleep)

# 1小时上传一次
scheduler.enter(周期(单位秒), 1, 函数名, ())
scheduler.run()

def 函数():
  scheduler.enter(周期(单位秒), 1, 函数名, ())

# 日志篇 #
# 创建日志的记录等级设
logging.basicConfig(level=logging.DEBUG)
# 创建日志记录器，指明日志保存的路径，每个日志文件的最大值，保存的日志文件个数上限
# 1024 * 1024 = 1048576
log_handle = RotatingFileHandler("*.txt", maxBytes=10240000, backupCount=5)
# 创建日志记录的格式
formatter = logging.Formatter("'%(asctime)s - %(levelname)s - pid: %(process)d - tid: %(thread)d - %(message)s'")
# 为创建的日志记录器设置日志记录格式
log_handle.setFormatter(formatter)
# 为全局的日志工具对象添加日志记录器
logging.getLogger().addHandler(log_handle)

# 调用接口统计日志, 每天归档
# 创建日志记录器，指明日志保存的路径，每个日志文件的最大值，保存的日志文件个数上限
# 一天一个日志
log_handle_qianzhi = TimedRotatingFileHandler("*.txt", when='MIDNIGHT', interval=1, backupCount=1)
# 备份后缀.2023-10-18
log_handle_qianzhi.suffix = "%Y-%m-%d"
# 创建日志记录的格式
formatter_qianzhi = logging.Formatter("%(asctime)s > %(message)s")
# 为创建的日志记录器设置日志记录格式
log_handle_qianzhi.setFormatter(formatter_qianzhi)
# 为全局的日志工具对象添加日志记录器
logging.getLogger('qianzi').addHandler(log_handle_qianzhi)
```
