# Projects
1. remove background (maintainer: 橘子)：https://github.com/Furiends/furiends-ai-remove-background
2. animal adoption system(maintainer: DA team)https://github.com/Furiends/furiends-ai-animal-adoption-system
3. pet classifier (maintainer: 乱乱): https://github.com/Furiends/furiends-ai-pet-classifier

# Code style
1. docstrings: functions都需要(vs code有相应插件)
```
def function_with_types_in_docstring(param1, param2):
    """Example function with types documented in the docstring.

    `PEP 484`_ type annotations are supported. If attribute, parameter, and
    return types are annotated according to `PEP 484`_, they do not need to be
    included in the docstring:

    Args:
        param1 (int): The first parameter.
        param2 (str): The second parameter.

    Returns:
        bool: The return value. True for success, False otherwise.

    .. _PEP 484:
        https://www.python.org/dev/peps/pep-0484/

    """
```
2. [naming convention](https://www.scutmath.com/GoogleStyleGuides/pyguide/index.html#guidelines-derived-from-guidos-recommendations:~:text=py%22%20%22%24%40%22.-,3.16.4%20Guidelines%20derived%20from%20Guido%E2%80%99s%20Recommendations,lower_with_under,-While%20Python%20supports)

![naming convention](https://github.com/Furiends/AI/blob/main/imges/naming.png)
3. 完善的README.md文档  
4. 完善的requirements.txt文档，请注明版本：
```
python>=3.9
torch>=1.7.0
...
```
5. git commit的内容是有实际含义的。

# Code review
```
git clone repo -> create branch -> coding -> pull request -> 微信群发送信息通知姐妹们有代码需要review -> 至少需要两位姐妹review代码 -> 第二位姐妹完成代码review后，由第二位姐妹执行merge步骤
```
