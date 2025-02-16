import subprocess
import time
import pygetwindow as gw
import pyautogui

def launch_chrome_instances(num_instances=3):
    """启动多个 Chrome 实例，并调整窗口大小和位置"""
    chrome_paths = [
        "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe",  # Windows 默认路径
        "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",  # macOS 默认路径
        "/usr/bin/google-chrome"  # Linux 默认路径
    ]
    
    # 选择可用的 Chrome 路径
    chrome_path = next((path for path in chrome_paths if Path(path).exists()), None)
    if not chrome_path:
        print("未找到 Chrome 可执行文件，请检查安装路径。")
        return
    
    chrome_windows = []
    for _ in range(num_instances):
        subprocess.Popen([chrome_path, "--new-window", "https://www.google.com"])  # 打开新窗口
        time.sleep(1)  # 等待窗口打开
    
    time.sleep(3)  # 等待所有窗口加载
    
    # 获取所有 Chrome 窗口
    all_windows = gw.getWindowsWithTitle("Google Chrome")
    
    if len(all_windows) < num_instances:
        print("未能成功打开所有 Chrome 窗口")
        return
    
    screen_width, screen_height = pyautogui.size()
    window_width = screen_width // num_instances
    window_height = screen_height
    
    # 平铺窗口
    for i, win in enumerate(all_windows[:num_instances]):
        if win.isMinimized:
            win.restore()
        win.moveTo(i * window_width, 0)
        win.resizeTo(window_width, window_height)
    
    print(f"成功打开 {num_instances} 个 Chrome 实例，并平铺窗口。")

if __name__ == "__main__":
    launch_chrome_instances(3)
