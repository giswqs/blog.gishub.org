---
title: "Google Colab Tips and Tricks"
datePublished: Tue Feb 02 2021 15:51:35 GMT+0000 (Coordinated Universal Time)
cuid: ckko6jbdy03ui1es1f3i05zy0
slug: google-colab-tips-and-tricks
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1612281057768/aH0ajIY70.png
tags: python, programming-tips, geemap

---

%[https://youtu.be/fmtB2PzqXVY]

1. **Scratchpad Notebook:**  https://colab.research.google.com/notebooks/empty.ipynb
2. **Open Notebooks from GitHub:**  simply replace github.com with githubtocolab.com. It will redirect you to a Colab notebook.
3. **Timing Execution of Cell:  **hover over the cell run icon and you will get an estimate of the execution time taken
4. **Run part of a cell: **Click `Runtime -> Run Selection` button or using the keyboard shortcut `Ctrl + Shift + Enter`
5. **Most commonly used shortcuts:** 
    - Run cell (`Ctrl + Enter`)
    - Run cell and add new cell below (`Alt + Enter`)
    - Run cell and goto the next cell below (`Shift + Enter`)
    - Comment current line (`Ctrl + /`)
6. **Jupyter Notebook Keyboard Shortcuts:** Click Tools -> Keyboard shortcuts or Just add `Ctrl + M` before whatever keyboard shortcut you were using in Jupyter. For example
    - add a cell above (`Ctrl + M + A`)
    - Add a cell below  (`Ctrl + M + B`)
    - Change cell to code (`Ctrl + M + Y`)
    - Change cell to markdown (`Ctrl + M + M`)
7. **Jump to Class definition:** press `Ctrl` and then clicking a class name
8. **Run bash commands:**
    - Download dataset from the web with `!wget <ENTER URL>`
    - Install libraries with `!pip install <LIBRARY>`
    - Clone a git repository with `!git clone <REPOSITORY URL>`
    - Change directory with `!cd`
9. **Mount your Google Drive to Colab:**
```
from google.colab import drive
drive.mount('/content/gdrive')
```
10. To upload a file (or several) from your computer, run:
```
from google.colab import files
files.upload()
```
11. To download a file, run:
```
from google.colab import files
files.download('path/to/your/file')
```
12. Run R programs in Google Colab:
You can use R programming language in Google Colab by going to  [https://colab.to/r](https://colab.to/r). It will open a new notebook with R set as the kernel instead of Python.
13. **“Open in Colab” Badge:** You can add a ‘Open in Colab’ badge to your **README.md** or jupyter notebooks using the following markdown code: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/notebooks/basic_features_overview.ipynb)
`[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/notebooks/basic_features_overview.ipynb)`
14. Run Rust programs in Google Colab:
Go to [https://colab.to/rust](https://colab.to/rust). Then rust the first cell. Reload the page. Skip the first and run other cells, or write your own codes.

**References:**
-  [Google Colab Tips for Power Users](https://amitness.com/2020/06/google-colaboratory-tips/)
-  [10 tricks for a better Google Colab experience](https://towardsdatascience.com/10-tips-for-a-better-google-colab-experience-33f8fe721b82) 