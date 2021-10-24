# RL and Advanced DL: Домашнее задание 1

Первое ДЗ связано с обучением с подкреплением, и оно придумано для ситуации, когда
нейронные сети ещё не нужны, и пространство состояний в целом достаточно маленькое,
чтобы можно было обучить хорошую стратегию методами TD-обучения или другими
методами обучения с подкреплением.

> ![](backjack.png)


## Часть первая, с блекджеком и стратегиями

Мы будем обучаться играть в очень простую, но знаменитую и популярную игру: блекджек.

Правила блекджека достаточно просты; давайте начнём с самой базовой версии, которая
реализована в OpenAI Gym:

```
➢ численные значения карт равны от 2 до 10 для карт от двойки до десятки, 10 для
валетов, дам и королей;
➢ туз считается за 11 очков, если общая сумма карт на руке при этом не превосходит
21 (по-английски в этом случае говорят, что на руке естьusable ace), и за 1 очко,
если превосходит;
➢ игроку раздаются две карты, дилеру — одна в открытую и одна в закрытую;
➢ игрок может совершать одно из двух действий:
  ○ hit — взять ещё одну карту;
  ○ stand— не брать больше карт;
➢ если сумма очков у игрока на руках больше 21, он проигрывает (bust);
➢ если игрок выбирает stand с суммой не больше 21, дилер добирает карты, пока
сумма карт в его руке меньше 17;
➢ после этого игрок выигрывает, если дилер либо превышает 21, либо получает
сумму очков меньше, чем сумма очков у игрока; при равенстве очков объявляется
ничья (ставка возвращается);
➢ в исходных правилах есть ещё дополнительный бонус заnatural blackjack: если
игрок набирает 21 очко с раздачи, двумя картами, он выигрывает не +1, а +1.
(полторы ставки).
```
Именно этот простейший вариант блекджека реализован в OpenAI Gym:

https://github.com/openai/gym/blob/master/gym/envs/toy_text/blackjack.py

1. Рассмотрим очень простую стратегию: говорить stand, если у нас на руках
    комбинация в 19, 20 или 21 очко, во всех остальных случаях говорить hit.
    Используйте методы Монте-Карло, чтобы оценить выигрыш от этой стратегии.
2. Реализуйте метод обучения с подкреплением без модели (можно Q-обучение, но
    рекомендую попробовать и другие, например Monte Carlo control) для обучения
    стратегии в блекджеке, используя окружение Blackjack-v0 из OpenAI Gym.
3. Сколько выигрывает казино у вашей стратегии? Нарисуйте графики среднего
    дохода вашего метода (усреднённого по крайней мере по 100000 раздач, а лучше
    больше) по ходу обучения. Попробуйте подобрать оптимальные гиперпараметры.

## Часть вторая, удвоенная

В базовый блекджек, описанный в предыдущем разделе, обыграть казино вряд ли
получится. Но, к счастью, на этом история не заканчивается. Описанные выше правила
были упрощёнными, а на самом деле у игрока есть ещё и другие возможности.
Реализовывать split может оказаться непросто, поэтому давайте ограничимся удвоением
ставки. Итак, у игрока появляется дополнительное действие:

```
➢ double— удвоить ставку; при этом больше действийделать нельзя, игроку
выдаётся ровно одна дополнительная карта, а выигрыш или проигрыш
удваивается.
```
4. Реализуйте новый вариант блекджека на основе окружения Blackjack-v0 из OpenAI
    Gym, в котором разрешено удвоение ставки.
5. Реализуйте метод обучения с подкреплением без модели для этого варианта,
    постройте графики, аналогичные п.2.

## Часть третья, в главной роли — Дастин Хоффман

А теперь давайте вспомним, как играют в блекджек настоящие профессионалы. Дело в
том, что в оффлайн-казино обычно не перемешивают колоду после каждой раздачи — это
слишком замедляло бы игру. После раздачи карты просто раздаются дальше с верха
колоды до тех пор, пока карт не останется слишком мало, и только тогда колода


перемешивается; давайте для определённости считать, что наше казино будет
перемешивать колоду, в которой осталось меньше 15 карт.

Думаю, у вас уже возникла в голове эта картинка:

Действительно, если вы будете запоминать, какие карты уже вышли, у вас будет
информация о том, какие карты ещё остались, а это позволяет лучше понять, когда нужно
удваивать ставку или делать split, а когда лучше не стоит. В настоящем казино могут
раздавать карты сразу из нескольких колод, и заслуга Rain Man’а была в том, что он смог
считать карты в шести колодах одновременно. Но мы с вами вооружены компьютерами,
так что подсчёт можно считать автоматическим.

6. Реализуйте вариант окружения Blackjack-v0 из предыдущей части (с удвоением), в
    котором игрок имеет возможность “считать карты” в колоде. Это можно сделать
    разными способами; возможно, вам поможетстатья википедиио блекджеке(а
    возможно, и нет).
7. Реализуйте метод обучения с подкреплением без модели для этого варианта,
    постройте графики, аналогичные п.2.

## Часть четвёртая, опциональная

Ну и напоследок ещё парочка опциональных заданий за дополнительные баллы.

8. Реализуйте поиск стратегии в блекджеке с известной моделью из первой части,
    решив уравнения Беллмана для V* или Q*. Для этого вам придётся сначала
    оценить параметры модели, т.е. найти или обучить вероятности переходов между
    состояниями.
9. Реализуйте вариант из второй или третьей части, в котором есть ещё возможность
    делатьsplit: в случае, когда игроку пришли две одинаковыекарты, он может
    разбить руку на две, внести ещё одну ставку и продолжать играть две руки сразу
    (как будто за двоих игроков). Скорее всего, обыграть казино получится только в
    варианте с разрешённым split’ом и подсчётом карт; если получится, это будет
    отличное завершение проекта!

