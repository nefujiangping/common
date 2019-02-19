##Tutorials:
- Refer to this blog: http://blog.csdn.net/jolinxia/article/details/77508435
- If you encounter some problems when following the steps, go to the NOTE section

##Outline
- Step 1: Create a new conda virtual env
```
    conda create -n py27env python=2.7 pip
    source activate py27env
    # use "source deactivate" to deactivate the env
```

- Step 2: Install pyrouge by conda from 3rd party
```
    conda install -c auto pyrouge
```

- Step 3: Download ROUGE-1.5.5 from its closed website by using a time machine (ask Einstein or go to Github), then copy ROUGE-1.5.5 to ~/rouge_path
```
    cp RELEASE-1.5.5 ~/rouge/
```

- Step 4: Set ROUGE path for pyrouge
```
    pyrouge_set_rouge_path ~/rouge/RELEASE-1.5.5
```

- Step 5: Install plugins for ROUGE-1.5.5
```
    sudo cpan App::cpanminus
    sudo cpanm XML::DOM
```

- Step 6: Deal with Wordnet exceptions for ROUGE-1.5.5 (f**k)
```
    cd ~/rouge/RELEASE-1.5.5/data/WordNet-2.0-Exceptions/
    ./buildExeptionDB.pl . exc WordNet-2.0.exc.db
    cd ../
    ln -s WordNet-2.0-Exceptions/WordNet-2.0.exc.db WordNet-2.0.exc.db
```

- Step 7: Deal with test code errors for pyrouge (f**k AGAIN!), In ```Rouge155_test.py``` file, you should modify two lines (refhttps://stackoverflow.com/a/41382391):
```
    vi ~/.anaconda3/envs/py27env/lib/python2.7/site-packages/pyrouge/tests/Rouge155_test.py

    modify
    "pyrouge_write_config_file.py -m {m} -s {s} " 
    to
    "pyrouge_write_config_file -m {m} -s {s} " 

    And, modify

    "pyrouge_evaluate_plain_text_files.py -m {} -s {} -sfp "
    to
    "pyrouge_evaluate_plain_text_files -m {} -s {} -sfp "
```

- Step 8: Run test until "OK" appears.
```
    python -m pyrouge.test

    Ran 10 tests in 10.583s
    OK
    Done. 
```
## NOTE:
- 1.ROUGE-1.5.5 can be obtained [here](https://github.com/andersjo/pyrouge)(in ```tools``` directory)
- 2.when using ```pip install pyrouge``` command line, you may get 'Failed to connect to ...' ERROR. Change the source of pip is a solution:
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyrouge
```
or change the configuration of pip:
```~/.config/pip/pip.conf``` (Linux), ```%APPDATA%\pip\pip.ini``` (Windows10), ```$HOME/Library/Application Support/pip/pip.conf``` (macOS)
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

- 3.In Step 4 command ```pyrouge_set_rouge_path /path/to/ROUGE/dir``` will write a file in ```~/.pyrouge/settings.ini```, content of which is:
```
[pyrouge settings]
home_dir = /path/to/ROUGE/dir
```
- 4.Also, in Step 5, you may get 'Failed to connect to ...' ERROR. Then change the mirror, using cpan with command line ```o conf urllist pop/push``` \
```cpanm XML::DOM --mirror some-mirror-link``` mirror links can be found [here](http://mirrors.ustc.edu.cn/CPAN/SITES.html)

- 5.In Step 6, ```ln -s WordNet-2.0-Exceptions/WordNet-2.0.exc.db WordNet-2.0.exc.db``` may get an ERROR, because of existing file ```WordNet-2.0.exc.db``` in the current directory. You can rename or remove ```WordNet-2.0.exc.db```, and then use this command.








