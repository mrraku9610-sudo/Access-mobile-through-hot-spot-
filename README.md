# Access-mobile-through-hot-spot-
Can access mobile 
import subprocess
import time

def run_adb(*args):
    result = subprocess.run(["adb", *args], capture_output=True, text=False)
    return result.returncode, result.stdout, result.stderr

# Connect to target phone on the hotspot
ip = "192.168.43.167"
code, out, err = run_adb("connect", f"{ip}:5555")
print("connect:", code, out.decode("utf-8", "ignore"), err.decode("utf-8", "ignore"))

# List devices
code, out, err = run_adb("devices")
print("devices:", code)
print(out.decode("utf-8", "ignore"))

# Example: open app or shell command
code, out, err = run_adb("shell", "wm", "size")
print("screen size:", code, out.decode("utf-8", "ignore"))

# Example: tap on screen
# 540, 1200 are just example coordinates
code, out, err = run_adb("shell", "input", "tap", "540", "1200")
print("tap:", code, out.decode("utf-8", "ignore"))

# Example: take a screenshot and save it on your PC
code, out, err = run_adb("exec-out", "screencap", "-p")
if code == 0:
    with open("screen.png", "wb") as f:
        f.write(out)
    print("screenshot saved as screen.png")
else:
    print("screenshot failed:", err.decode("utf-8", "ignore"))
