## Index
[Introduction](#Introduction)   
[Structure of Package](#Structure)  
[how to use](#Usage)     
[Result](#Result)
***

## introduction
Our goal is to make the program compatible on all dies, not on one die. 

For running a program, just only 'run.py' file should make the program run. - Make each analysis step into a module and load it.
### contributors: If your have any questions,please contact us at the following email.

| name | e_mail | 
| --- | --- |
| 나유진 | [nayoujin2002@hanyang.ac.kr] |
| 창영곤 | [changyongkun@hanyang.ac.kr] |
| 박소연 | [psy030412@hanyang.ac.kr] |
| 박승우 | [seungwo7@hanyang.ac.kr] |




## Structure
+ dat : data files
+ doc : notebook (explaining how to use our package)
+ res : Show results(CSV, jpgs) 
+ src
   + device_waferno_find_xml: Searches directories for XML files related to specific devices and wafer numbers, filtering based on user-defined criteria.
   + flat_transmission： plot flatten wavelength sweep graph
   + install_missing_modules: Ensures required Python modules are installed by reading a requirements.txt file and installing any missing modules using pip.
   + ivcurve：Extract the current value at -1V on the IV graph and the current value at 1V on the IV graph. And draw the I-V relationship graph
   + pandas_frame：Extract spectral and IV data from multiple XML files, process and fit the data, and finally organize the processed data into a Pandas data frame
   + process_data: Processes data from XML files to generate graphs of IV curves and transmission spectra, saves the processed data to an Excel file, and stores the graphs as JPG files.
   + ref_transmission：Automatically process multiple XML files, extract spectral data, perform polynomial fits, generate corresponding graphs, and save these graphs as JPG files
   + requirements.txt: Required Module names
   + transmission：Automatically process multiple XML files, extract spectral data and generate corresponding graphs, and save these graphs as JPG files for subsequent analysis and use
+ .gitignore：Ignore unnecessary files
+ README：Documentation on development
+ run：main program (Processes data based on device and wafer numbers configurations, installs necessary modules, and generates graphs.)

## Usage
### Library function required to execute code
```python
import os
import xml.etree.ElementTree as ET
import numpy as np
import matplotlib.pyplot as plt
import lmfit
import pandas as pd
import src.pandas_frame
import warnings
from   scipy.interpolate import UnivariateSpline
from   src.ivcurve import plot_iv_data
from   src.transmission import plot_transmission_spectra_all
from   src.ref_transmission import plot_transmission_spectra
from   src.flat_transmission import plot_flat_transmission_spectra
```
### Execution code for executing the entire program
```python
from src.install_missing_modules import install_missing_modules
from src.process_data import process_data

def main():
    device = 'LMZC'  # 디바이스 설정('LMZC', 'LMZO', 'ALL') *대소문자 구별X
    wafer_nos = 'D07'  # 웨이퍼 번호 설정 ('D07', 'D08', 'D23', 'D24', 'ALL')

    # 필요한 경우 모듈 설치
    install_missing_modules('requirements.txt')

    # 데이터 처리 및 그래프 생성
    process_data(device, wafer_nos)

if __name__ == "__main__":
    main()

```

Running `run.py` will display these messages.

```
xml.etree.ElementTree 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...
lmfit 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...
numpy 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...
matplotlib 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...
scipy 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...
pandas 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...
```
it means that the modules stored in the requirements have been automatically installed by following codes


```python
import os
import subprocess

def install_missing_modules(filename='requirements.txt'):
    # 현재 파일의 경로에서 requirements.txt 파일의 절대 경로 설정
    current_dir = os.path.dirname(os.path.abspath(__file__))
    abs_file_path = os.path.normpath(os.path.join(current_dir, filename))

    if os.path.exists(abs_file_path):
        with open(abs_file_path) as f:
            modules = f.read().splitlines()

        for module in modules:
            try:
                __import__(module)
                print(f"{module} 모듈이 설치되어 있습니다. 계속 진행합니다.")
            except ImportError:
                print(f"{module} 모듈이 설치되어 있지 않습니다. 자동으로 설치합니다...")
                subprocess.call(['pip', 'install', module])
    else:
        print(f"파일 '{abs_file_path}'을(를) 찾을 수 없습니다. 패키지 설치를 스킵합니다.")
```

Then, you can modify the file you want to analyze by editing the code of 'run.py' like this.

```python
def main():
    device = 'ALL'  # 디바이스 설정('LMZC', 'LMZO', 'ALL') *대소문자 구별X
    wafer_nos = 'D23'  # 웨이퍼 번호 설정 ('D07', 'D08', 'D23', 'D24', 'ALL')
```

Finally, CSV file of analysis data and graphs will be saved automatically.
The stored analysis files are saved based on the time of storage, so previous analysis files are not deleted.  

![saved files](https://github.com/nayoujin2002/programing_fic/blob/main/%EC%A0%80%EC%9E%A5.PNG)


***

## Result

![Example of Analysis data CSV](https://github.com/nayoujin2002/programing_fic/blob/main/%EC%BA%A1%EC%B2%98.PNG)
![Example of Analysis graph](https://github.com/nayoujin2002/programing_fic/blob/main/HY202103_D07_(0%2C0)_LION1_DCM_LMZC.jpg)
