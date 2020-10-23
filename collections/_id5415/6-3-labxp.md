---
layout: course-page
title: "Analytics Notebook"
permalink: /module6/labxp
description: "Prototyping Connected Products - Lab Experiment 6"
labxp-of: id5415-6
introduction: In this last Lab Experiment, we will use a Jupyter Notebook to guide us through building common data charts using data from Raspberry Pis and lightbulbs.
technique:
metrics:
report:
---

---

* Do not remove this line (it will not be displayed)
{:toc}

---


>When opening VS Code, we first load the Python environment.

Let's update package of the data-centric design lab.

```bash
pip uninstall dcd-sdk
pip install dcd-sdk
```

To use Jupyter Notebook on our machine, we install it with pip.

```bash
pip install jupyter
```

Download the following notebook and save it a the root of your project directory: [labxp6.ipynb](/assets/labxp6.ipynb)

In the Terminal of VS code, start Jupyter with the following command. Running this command should redirect you to your favourite web browser and open the Jupyter web interface. Otherwise, a link shows up in the Terminal, you can copy and paste it in your web browser.

```bash
python -m jupyter notebook
```

By default, Jupyter lists the files in the current directory which should be your project directory. Click on the file labxp6.ipynb to start the notebook.

If this is your first experience with a Jupyter Notebook, you can find a [quick tour here](https://www.youtube.com/watch?v=jZ952vChhuI).

For this experiment, we use the Jupyter Notebook to report. There is no need for a separate markdown file.