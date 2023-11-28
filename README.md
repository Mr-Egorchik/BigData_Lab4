# BigData_Lab4

Читоркин Егор, 6133\
Задания выполнялись на языке Scala

### Работа с Zookeeper через консоль

Сначала про работу в консоли.

Сначала мы посмотрели содержимое корневого каталога [2], а также дочерние узлы /zookeeper [3]. Далее было создан узел /mynode с содержимым 'first_version' [4], после чего произведена проверка его создания [5]. Далее было получено значение этого узла[6] и выведена метаинформация об узле [7].

![](/img/1.jpg)

Далее было изменено содержимое узла /mynode [8] и произведена проверка изменения [9]. Далее выведена обновленная метаинформация об узле [10]

![](/img/2.jpg)

Далее в узле /mynode были создано дочерние sequentional-узлы [11-12]

![](/img/3.jpg)

Далее был создан узел /mygroup. Далее через новые консоли были созданы эфимерные узлы

![](/img/4.jpg)

![](/img/5.jpg)

С основной консоли проведена проверка содержимого группы

![](/img/6.jpg)

Далее из консоли grue получены данные узла bleen

![](/img/7.jpg)

Далее удалили узел bleen, дождались когда он удалится [15-17]

![](/img/8.jpg)

Далее создан узел /myconfig со значением 'sheep_count=1' [23], проверка содержимого [24]

![](/img/9.jpg)

Из другой консоли установлен watch-trigger на узел

![](/img/10.jpg)

Далее из основной консоли вызываем изменение значения в узле, в другой консоли сработал триггер об изменении значения.

![](/img/11.jpg)

Далее удален узел /myconfig

![](/img/12.jpg)

### Задача о философах

Каждый философ - это Thread. Вилки - это мьютексы (двоичные семафоры). Я взял количество мест равным количеству философов (7). Философ кушает и думает.
В методе eat() создается эфимерный узел, после чего с помощью метода aquire() филосов берет правую и левую вилку. Далее философ трапезничает (поток засыпает). После трапезы философ последовательно кладет каждую из вилок (освобождает семафоры) с помощью метода release(). После этого философ начинает думать. В методе think() эфимерный узел удаляется и поток засыпает - философ ушел думать.

Скрин с мониторингом количества философов

![](/img/13.jpg)

### Двухфазный коммит

Реализован второй метод. Есть 2 вида потоков: Coordinator и Worker.

Coordinator создает свой эфимерный узел, после чего ожидает, пока worker'ы создадут свои узлы и проголосуют. Worker'ы создают эфимерные узлы как дочернии к узлу координатора, значение узла - commit или abort - устанавливается рандомно. После получения голосов Coordinator принимает решение (относительное большинство), рассылает его дочерним узлам.
