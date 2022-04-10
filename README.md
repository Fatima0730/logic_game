# Logic_game
## Logic game
Для этой программы понадобились библиотеки **Pygame** и **Tkinter**. `import *`  представляет все функции в библиотеке Tkinter
```python
from tkinter import *
import pygame
```

Создаем окно с текстом `Tk()`, редактируем его с помощью элементов `fg`, `font`  (определяем цвет в формате _rgb_, размеры, шрифт) и выводим в окно tk `pack()`
```python
window = Tk()
window.title('Logic quiz') # текст отображатеся на верхней части окна

label = Label(text='''
    Добро пожаловать в логическую викторину!
    В игре всего 5 заданий, каждый из которых
    оценивается на 1 балл. В конце викторины
    Вы сможете получить ответы на все вопросы
    и свой конечный счет
    У вас есть всего одна попытка, чтобы ответить на вопрос.
    Удачи!''', font="Arial 24", fg='#316879')
label.pack()
```

Теперь надо определить местоположение самого окна на экране компьютера, используя оси координаты  _х_ и _у_ , определить размеры окна `geometry` и вывести на экран `mainloop()`
```python
x = (window.winfo_screenwidth() - window.winfo_reqwidth()) / 4
y = (window.winfo_screenheight() - window.winfo_reqheight()) / 5
window.wm_geometry("+%d+%d" % (x, y))
window.geometry('800x450')
window.mainloop()
```

Теперь создаем кнопку `Button()`, с помощью которой можно закрыть предыдущее окно `window.destroy()`, а также при нажатии на нее будет звуковой эффект. PyCharm начинает искать миксер `pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')`. Чтобы звуковой эффект играл не до окончания игры, а всего олин раз, надо в `pygame.mixer.music.play()` указать _0_. 
Редактируем ее (обязательно добавляем команду _sound_, чтобы при нажатии работала данная функция) и выводим так же с помощью `pack()` в первое окно `Button(window)`. Также надо определить местоположение кнопки `but.pack(side='bottom')`
```python
def sound():
    pygame.init() # инициализация pygame
    pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')
    pygame.mixer.music.play(0)
    window.destroy()
but = Button(window, text="Начать викторину", width=40, height=2, command=sound, fg='#4B4B4B')
but.pack(side='bottom')
```

Теперь таким же способом редактируем первый вопрос
```python
window = Tk()
window.title('Logic quiz')
label = Label(text='''Телефон вместе с чехлом стоит 110 долларов. Телефон дороже чехла на 100 долларов.
    Сколько стоит чехол на телефон?''', font="Arial 21", fg='#316879')
label.config(bd=30)
label.pack()

label1 = Label(text="Первый вопрос", font="Arial 19", width=80, height=2, fg='#316879')
label1.config(bg='#ced7d8')
label1.pack()

but = Button(window, text="10", width=40, height=2, command=close_window1, fg='#4B4B4B')
but.pack(side='bottom')
but1 = Button(window, text="5", width=40, height=2, command=close_window, fg='#4B4B4B')
but1.pack(side='bottom')
but2 = Button(window, text="100", width=40, height=2, command=close_window1, fg='#4B4B4B')
but2.pack(side='bottom')
but3 = Button(window, text="200", width=40, height=2, command=close_window1, fg='#4B4B4B')
but3.pack(side='bottom')

x = (window.winfo_screenwidth() - window.winfo_reqwidth()) / 10
y = (window.winfo_screenheight() - window.winfo_reqheight()) / 15
window.wm_geometry("+%d+%d" % (x, y))
window.geometry('950x650')
window.mainloop()
```
Для создания счетчика балла переменная `score` была добавленна в глобальную область видимости с помощью функции `global`. При правильном ответе работала функция `def close_window()`, которая была добавлена в команду кнопки с правильным ответом. В этой функции прибавлется балл `score += 1` и выводится в окно `scoreLabel.config(text='''Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17')`. В данную функцию добавлена еще одна функция _sound_, в которую добавлен другой миксер, а также добавлена функция `window.destroy()`. Эта же функция будет работать, когда пользователь нажмет на кнопку "Перейти к следующему вопросу", которая появляется только после ответа на вопрос. С помощью элемента `'disabled'` кнопки перестают функционировать после нажатия, что не позволяет пользоваелю отвечать на этот же вопрос повторно. 
```python
def close_window():
    pygame.init()
    pygame.mixer.music.load('ES_Multimedia 781 - SFX Producer.mp3')
    pygame.mixer.music.play(0)
    but['state'] = 'disabled'
    but1['state'] = 'disabled'
    but2['state'] = 'disabled'
    but3['state'] = 'disabled'
    global score
    score += 1
    scoreLabel.config(text='''
    Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17')
    label = Label(text='''
    Верно
    ''', font="Arial 19", fg='#7fe7dc')
    label.pack()
    def sound():
        pygame.init()
        pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')
        pygame.mixer.music.play(0)
        window.destroy()
    buttun = Button(window, text="Перейти к следующему вопросу", width=40, height=2, command=sound,
                    fg='#4B4B4B')
    buttun.pack(pady=5, padx=100)

```

Этим же принципом создается функция при неправильном ответе. Это функция помещена в команду трех других кнопок, то есть при нажатии на одну любую из этих кнопок, будет работать данная функция. 
```python
def close_window1():
    pygame.init()
    pygame.mixer.music.load('ES_MM Error 23 - SFX Producer.mp3')
    pygame.mixer.music.play(0)
    but['state'] = 'disabled'
    but1['state'] = 'disabled'
    but2['state'] = 'disabled'
    but3['state'] = 'disabled'
    label = Label(text='''Неверно''', font="Arial 19", fg='#f47a60')
    label.config(bd=30)
    label.pack()
    scoreLabel.config(text='''
    Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17') # в этом лучае балл не прибавляется, а просто выводится в окно
    def sound():
        pygame.init()
        pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')
        pygame.mixer.music.play(0)
        window.destroy()
    buttun = Button(window, text="Перейти к следующему вопросу", width=40, height=2, command=sound,
                    fg='#4B4B4B')
    buttun.pack(pady=5, padx=100)

```

Также вне функций мы добавляем текст и ранее набранные баллы, чтобы пользователь видел свои баллы в течении всей игры
```python
scoreLabel = Label(window, text='''
    Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17')
scoreLabel.pack()
```

И так добавляются все следующие вопросы
```python
# 2 window
window = Tk()
window.title('Logic quiz')
label = Label(text='''Запишите восемь восьмерок таким образом, чтобы в сумме получилась тысяча.
    Какой вариант верный? ''', font="Arial 21", fg='#316879')
label.config(bd=30)
label.pack()

label1 = Label(text="Второй вопрос", font="Arial 19", width=80, height=2, fg='#316879')
label1.config(bg="#ced7d8")
label1.pack()


def close_window():
    pygame.init()
    pygame.mixer.music.load('ES_Multimedia 781 - SFX Producer.mp3')
    pygame.mixer.music.play(0)
    but['state'] = 'disabled'
    but1['state'] = 'disabled'
    but2['state'] = 'disabled'
    but3['state'] = 'disabled'
    global score
    score += 1
    scoreLabel.config(text='''
    Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17')
    label = Label(text='Верно', font="Arial 19", fg='#7fe7dc')
    label.config(bd=30)
    label.pack()
    def sound():
        pygame.init()
        pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')
        pygame.mixer.music.play(0)
        window.destroy()
    buttun = Button(window, text="Перейти к следующему вопросу", width=40, height=2, command=sound,
                    fg='#4B4B4B')
    buttun.pack(pady=5, padx=100)


def close_window1():
    pygame.init()
    pygame.mixer.music.load('ES_MM Error 23 - SFX Producer.mp3')
    pygame.mixer.music.play(0)
    but['state'] = 'disabled'
    but1['state'] = 'disabled'
    but2['state'] = 'disabled'
    but3['state'] = 'disabled'
    label = Label(text='Неверно', font="Arial 19", fg='#f47a60')
    label.config(bd=30)
    label.pack()
    scoreLabel.config(text='''
    Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17')
    def sound():
        pygame.init()
        pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')
        pygame.mixer.music.play(0)
        window.destroy()
    buttun = Button(window, text="Перейти к следующему вопросу", width=40, height=2, command=sound,
                    fg='#4B4B4B')
    buttun.pack(pady=5, padx=100)


scoreLabel = Label(window, text='''
    Ваши баллы: ''' + str(score), fg='#4B4B4B', font='Arial 17')
scoreLabel.pack()
label.config(bd=30)

but = Button(window, text="888 + 88 + 8 + 8 + 8 + 8", width=40, height=2, command=close_window1, fg='#4B4B4B')
but.pack(side='bottom')
but1 = Button(window, text="88 + 88 + 888 + 8", width=40, height=2, command=close_window1, fg='#4B4B4B')
but1.pack(side='bottom')
but2 = Button(window, text="888 + 88 + 8 + 8 + 8", width=40, height=2, command=close_window, fg='#4B4B4B')
but2.pack(side='bottom')
but3 = Button(window, text="888 + 88 + 8 - 8 + 8 + 8", width=40, height=2, command=close_window1, fg='#4B4B4B')
but3.pack(side='bottom')

x = (window.winfo_screenwidth() - window.winfo_reqwidth()) / 10
y = (window.winfo_screenheight() - window.winfo_reqheight()) / 15
window.wm_geometry("+%d+%d" % (x, y))
window.geometry('950x650')
window.mainloop()
```

Для обьяснений заданий было создано пять функций (для вывода в разные окна разный текст). Принцип был тот же, но при нажатии на одну кнопку, переставала функционировать только она. 
```python
def close_window2():
    pygame.init()
    pygame.mixer.music.load('najatie-na-kompyuternuyu-knopku1.wav')
    pygame.mixer.music.play(0)
    window = Tk()
    window.title('Logic quiz')
    but2['state'] = 'disabled'
    label = Label(window, text='''В начале нужно выяснить сколько было членов семьи 4 года назад для этого нужно
        64-4*4=52, что меньше, чем дано в условии следовательно 4 года назад было 3 члена семьи
        68-4*3=56
        56- общий возраст членов семьи 4 года назад + возраст нового члена семьи. Новому члену семьи 56-53=3 года''',
                  font="Arial 21", fg='#316879')
    label.config(bd=30)
    label.pack()
```

Для обьяснений одного из вопросов была использована библиотека `tkscrolledframe`, которую я импортировала в самой функции. 
```python
from tkscrolledframe import ScrolledFrame
    root = Tk()
    root.geometry('1250x750')
    root.title('Logic quiz')
    frame_top = Frame(root, width=400, height=250)
    frame_top.pack(side="top", expand=1, fill="both")
    sf = ScrolledFrame(frame_top, width=380, height=240)
    sf.pack(side="top", expand=1, fill="both")
    sf.bind_arrow_keys(frame_top)
    sf.bind_scroll_wheel(frame_top)
    frame = sf.display_widget(Frame)
    l = Label(frame, text='''*text*''')
    l.pack()
    root.mainloop()
```

А также выводятся сами баллы
```python
scoreLabel = Label(text="Ваш конечный балл: " + str(score), font="Arial 19", width=70, height=2, fg='#4B4B4B')
scoreLabel.config(bg='#93D554')
scoreLabel.pack()
```
