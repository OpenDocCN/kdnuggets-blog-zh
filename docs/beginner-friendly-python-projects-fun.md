# 初学者友好的有趣 Python 项目！

> 原文：[https://www.kdnuggets.com/2022/10/beginner-friendly-python-projects-fun.html](https://www.kdnuggets.com/2022/10/beginner-friendly-python-projects-fun.html)

![初学者友好的有趣 Python 项目！](../Images/80a54cc8c53d4d41bed56d9ccfeb2cf9.png)

[Andrey Metelev](https://unsplash.com/@metelevan) 通过 Unsplash

我最近做了一篇关于在不到 5 分钟内构建 Python 项目的文章。所以我决定再做一篇，提供更多供初学者玩耍和测试技能的项目。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

如果你在职业生涯中没有乐趣，你很快会失去激情并开始讨厌它。像这样的项目不仅适合初学者，还能为你的学习或职业增添一点乐趣。

现在我们开始吧。

# 猜数字

在这个第一个项目中，我们将生成一个特定范围内的随机数字，用户需要通过提示来猜测。

用户猜错的次数越多，给出的提示就会越多——但这会减少他们的得分。

## 代码：

```py
""" Guess The Number """
import random
attempts_list = []
def show_score():
    if len(attempts_list) <= 0:
        print("There is currently no high score, it's yours for the taking!")
    else:
        print("The current high score is {} attempts".format(min(attempts_list)))
def start_game():
    random_number = int(random.randint(1, 10))
    print("Hello traveler! Welcome to the game of guesses!")
    player_name = input("What is your name? ")
    wanna_play = input("Hi, {}, would you like to play the guessing game? (Enter Yes/No) ".format(player_name))
    #Where the show_score function USED to be
    attempts = 0
    show_score()
    while wanna_play.lower() == "yes":
        try:
            guess = input("Pick a number between 1 and 10 ")
            if int(guess) < 1 or int(guess) > 10:
                raise ValueError("Please guess a number within the given range")
            if int(guess) == random_number:
                print("Nice! You got it!")
                attempts += 1
                attempts_list.append(attempts)
                print("It took you {} attempts".format(attempts))
                play_again = input("Would you like to play again? (Enter Yes/No) ")
                attempts = 0
                show_score()
                random_number = int(random.randint(1, 10))
                if play_again.lower() == "no":
                    print("That's cool, have a good one!")
                    break
            elif int(guess) > random_number:
                print("It's lower")
                attempts += 1
            elif int(guess) < random_number:
                print("It's higher")
                attempts += 1
        except ValueError as err:
            print("Oh no!, that is not a valid value. Try again...")
            print("({})".format(err))
    else:
        print("That's cool, have a good one!")
if __name__ == '__main__':
    start_game()
```

# 猜字谜

猜字谜的重点是选择一个单词，所以首先我们需要找到一个单词列表。在 StackOverflow 上，有一个包含 2400 多个单词的 JSON 文件。你可以在这里找到它：[randomlist](https://www.randomlists.com/data/words.json)。

使用这个 JSON 文件，将这些单词复制到一个 .py 文件中，并将其分配给变量 ‘words’。像这样：

```py
words = "aback","abaft","abandoned","abashed","aberrant","abhorrent"...
```

## 代码：

创建一个第二个 .py 文件，命名为 hangman.py，它将包含这些内容：

```py
""" Hangman """

#Imports
​​import random
from words import words
from hangman_visual import lives_visual_dict
import string

def get_valid_word(words):
    word = random.choice(words)  # randomly chooses something from the list
    while '-' in word or ' ' in word:
        word = random.choice(words)

    return word.upper()

def hangman():
    word = get_valid_word(words)
    word_letters = set(word)  # letters in the word
    alphabet = set(string.ascii_uppercase)
    used_letters = set()  # what the user has guessed

    lives = 7

    # getting user input
    while len(word_letters) > 0 and lives > 0:
        # letters used
        # ' '.join(['a', 'b', 'cd']) --> 'a b cd'
        print('You have', lives, 'lives left and you have used these letters: ', ' '.join(used_letters))

        # what current word is (ie W - R D)
        word_list = [letter if letter in used_letters else '-' for letter in word]
        print(lives_visual_dict[lives])
        print('Current word: ', ' '.join(word_list))

        user_letter = input('Guess a letter: ').upper()
        if user_letter in alphabet - used_letters:
            used_letters.add(user_letter)
            if user_letter in word_letters:
                word_letters.remove(user_letter)
                print('')

            else:
                lives = lives - 1  # takes away a life if wrong
                print('\nYour letter,', user_letter, 'is not in the word.')

        elif user_letter in used_letters:
            print('\nYou have already used that letter. Guess another letter.')

        else:
            print('\nThat is not a valid letter.')

    # gets here when len(word_letters) == 0 OR when lives == 0
    if lives == 0:
        print(lives_visual_dict[lives])
        print('You died, sorry. The word was', word)
    else:
        print('YAY! You guessed the word', word, '!!')

if __name__ == '__main__':
    hangman()
```

运行你的 hangman.py 文件，开始游戏吧！

# 石头、剪刀、布

石头、剪刀、布游戏使用了 random.choice()、if 语句和获取用户输入与这些函数配合：

+   用于生成石头、剪刀或布的随机函数。

+   验证函数用于检查你的动作是否有效

+   结果函数用于检查谁赢得了这一轮

+   记分员用于跟踪得分。

## 代码：

```py
# if not re.match("^[a-z]*$", input_str):
import random
import os
import re

os.system("cls" if os.name == "nt" else "clear")

while 1 < 2:
    print("\n")
    print("Rock, Paper, Scissors - Shoot!")
    userChoice = input("Choose your weapon [R]ock, [P]aper, or [S]cissors, [E]xit: ")
    if userChoice == "E":
        break
    if not re.match("[SsRrPp]", userChoice):
        print("Please choose a letter:")
        print("[R]ock, [S]cissors or [P]paper.")
        continue
    # Echo the user's choice
    print("You chose: " + userChoice)

    choices = ["R", "P", "S"]
    opponenetChoice = random.choice(choices)

    print("I chose: " + opponenetChoice)

    if opponenetChoice == str.upper(userChoice):
        print("Tie!")

    # if opponenetChoice == str("R") and str.upper(userChoice) == "P"

    elif opponenetChoice == "R" and userChoice.upper() == "S":
        print("Scissors beats rock, I win!")
        continue
    elif opponenetChoice == "S" and userChoice.upper() == "P":
        print("Scissors beats paper! I win!")
        continue
    elif opponenetChoice == "P" and userChoice.upper() == "R":
        print("Paper beat rock, I win! ")
        continue
    else:
        print("You win!")

```

# 结论

我希望这能帮助你走出舒适区，花 30 分钟来测试你的 Python 技能。我将创建另一篇文章，提供更多来自 YouTuber 的项目教程内容！

**[尼莎·阿里亚](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由职业技术作家。她特别关注提供数据科学职业建议或教程以及数据科学相关理论知识。她还希望探索人工智能如何及将如何有利于人类寿命的不同方式。她是一名热衷于学习的人员，寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关主题

+   [7 个适合初学者的项目，助您快速上手 ChatGPT](https://www.kdnuggets.com/2023/08/7-beginnerfriendly-projects-get-started-chatgpt.html)

+   [使用 tqdm 在 Python 中创建进度条以获得乐趣和利润](https://www.kdnuggets.com/2022/09/progress-bars-python-tqdm-fun-profit.html)

+   [简短有趣的课程，帮助您了解生成性人工智能](https://www.kdnuggets.com/short-and-fun-courses-to-get-you-up-to-speed-about-generative-ai)

+   [预测制作：初学者的 Python 线性回归指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [掌握 GPU：初学者的 GPU 加速 DataFrames 在 Python 中的指南](https://www.kdnuggets.com/2023/07/mastering-gpus-beginners-guide-gpu-accelerated-dataframes-python.html)

+   [初学者的 Python 机器学习指南](https://www.kdnuggets.com/beginners-guide-to-machine-learning-with-python)

![](../Images/eb105b2b614c4a6165d83fbc2b9711a1.png)

[](/news/subscribe.html)

[获取免费电子书《伟大的自然语言处理入门》以及《数据科学备忘单全集》和领先的关于数据科学、机器学习、人工智能与分析的新闻通讯，直接发送到您的邮箱。](/news/subscribe.html)

订阅即表示您接受 KDnuggets 的 [隐私政策](https://www.kdnuggets.com/news/privacy-policy.html)

* * *

[<= 上一篇文章](https://www.kdnuggets.com/2022/10/top-free-git-gui-clients-beginners.html)[下一篇文章 =>](https://www.kdnuggets.com/2022/10/top-posts-week-0926-1002.html)

### [最新文章](/news/index.html)

+   [如何跟踪 Python 中的内存分配](https://www.kdnuggets.com/how-to-trace-memory-allocation-in-python)

+   [如何在 R 中导入数据](https://www.kdnuggets.com/how-to-import-data-in-r)

+   [实践者应了解的 5 个机器学习 API](https://www.kdnuggets.com/top-5-machine-learning-apis-practitioners-should-know)

+   [谷歌、Snowflake 和微软的最新专业技术证书](https://www.kdnuggets.com/new-professional-tech-certificates-from-google-snowflake-microsoft)

+   [5 个数据科学的隐藏宝藏 Python 库](https://www.kdnuggets.com/5-hidden-gem-python-libraries-for-data-science)

+   [NumPy 在线性代数应用中的作用](https://www.kdnuggets.com/numpy-for-linear-algebra-applications)

|

## 热门文章

|

+   [微软提供的 4 个入门级证书，助您获得热门工作](https://www.kdnuggets.com/4-entry-level-certificates-from-microsoft-to-land-in-demand-jobs)

+   [5个数据科学的隐藏宝石Python库](https://www.kdnuggets.com/5-hidden-gem-python-libraries-for-data-science)

+   [每个数据工程师应该了解的10个内置Python模块](https://www.kdnuggets.com/10-built-in-python-modules-every-data-engineer-should-know)

+   [如何使用 Pandas 有效管理分类数据](https://www.kdnuggets.com/how-to-manage-categorical-data-effectively-with-pandas)

+   [掌握数据工程的项目创意](https://www.kdnuggets.com/project-ideas-to-master-data-engineering)

+   [使用 FastAPI 构建 ML 驱动的 Web 应用](https://www.kdnuggets.com/using-fastapi-for-building-ml-powered-web-apps)

+   [我参加了 Udacity 的 Google 免费 A/B 测试课程：我学到了什么](https://www.kdnuggets.com/i-took-udacitys-free-a-b-testing-course-by-google-heres-what-i-learned)

+   [停止为课程付费，免费学习](https://www.kdnuggets.com/stop-paying-for-courses-and-learn-for-free)

+   [本地使用 FLUX.1](https://www.kdnuggets.com/using-flux-1-locally)

+   [5个常见的数据科学错误及其避免方法](https://www.kdnuggets.com/5-common-data-science-mistakes-and-how-to-avoid-them)

* * *

© 2024 [Guiding Tech Media](https://www.guidingtechmedia.com/)   |   [关于](/about/index.html)   |   [联系](/contact.html)   |   [广告]((https://mailchi.mp/kdnuggets/media-kit) |   [隐私](/news/privacy-policy.html)   |   [服务条款](/terms-of-service.html)

由 Nisha Arya 于 2022年10月3日 发布
