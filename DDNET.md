Разбираюсь с дискордом :
![[Pasted image 20241116204942.png]]
****
![[Pasted image 20241116205331.png]]
![[Pasted image 20241116210734.png]]
![[Pasted image 20241116210758.png]]
![[Pasted image 20241116210937.png]]
![[Pasted image 20241116211045.png]]
![[Pasted image 20241116212208.png]]
![[Pasted image 20241116212630.png]]
![[Pasted image 20241116212720.png]]
тут 200 я где то еще видел что тики может их кол во в секунду связанно с 200

./game/client/gameclient.cpp
514:            m_HammerInput.m_Fire = (m_HammerInput.m_Fire + 1) | 1;

![[Pasted image 20241116222940.png]]
инпут дата
![[Pasted image 20241116223118.png]]
266:                    pDummyInput->m_WantedWeapon = m_aInputData[g_Config.m_ClDummy].m_WantedWeapon;
269:                            pDummyInput->m_Fire += m_aInputData[g_Config.m_ClDummy].m_Fire - m_aLastData[g_Config.m_ClDummy].m_Fire;
271:                    pDummyInput->m_NextWeapon += m_aInputData[g_Config.m_ClDummy].m_NextWeapon - m_aLastData[g_Config.m_ClDummy].m_NextWeapon;
272:                    pDummyInput->m_PrevWeapon += m_aInputData[g_Config.m_ClDummy].m_PrevWeapon - m_aLastData[g_Config.m_ClDum


308:            Send = Send || m_aInputData[g_Config.m_ClDummy].m_Jump != m_aLastData[g_Config.m_ClDummy].m_Jump;
309:            Send = Send || m_aInputData[g_Config.m_ClDummy].m_Fire != m_aLastData[g_Config.m_ClDummy].m_Fire;
310:            Send = Send || m_aInputData[g_Config.m_ClDummy].m_Hook != m_aLastData[g_Config.m_ClDummy].m_Hook;

![[Pasted image 20241116225010.png]]
Тут юзается INPUT_STATE_MASK которая в питоне на 0x3f выставляется(только в питоне инитится)

![[Pasted image 20241116225702.png]]
Странная фигня при попытке поиска по этой фигне не че не выдает.
![[Pasted image 20241116230020.png]]
она инитится только тут получается.
[[g_Config]] юзается много почти везде где рисуется что то и с управлением.
![[Pasted image 20241117193103.png]]
[[ban]] и [[kick]] в [[DDNET]].
![[Pasted image 20241117194129.png]]
[[m_ServerInfoNeedsUpdate]]
![[Pasted image 20241117195819.png]]
Этот кусок можно заменить на [[Chunk.front()]].щас читал функции вектор front это первый элемент возвращает вектора.
![[Pasted image 20241117210513.png]]
![[Pasted image 20241117210526.png]]
на [[AddRaw]] остановился.
![[Pasted image 20241120204034.png]]
![[Pasted image 20241120204221.png]]
 ![[Pasted image 20241121211143.png]]
 Щас буду разбираться что такое симофоры.
 ![[Pasted image 20241124171806.png]]
 ![[Pasted image 20241124173346.png]]
 ![[Pasted image 20241130144307.png]]
 ![[Pasted image 20241130155130.png]]
 ![[Pasted image 20241201120436.png]]
 ![[Pasted image 20241201133035.png]]
 необычно юзаются указатели на функции в функции которая скорее сего их вызывает вместо того чтобы просто их вызвать
 ![[Pasted image 20241201134728.png]]
 ![[Pasted image 20241201135308.png]]
 [[RateLimitPlayer]] ограничение скорости игрока.
 ![[Pasted image 20241201144027.png]]
 надо разобраться в устройтве покаовки и распоковки.![[Pasted image 20241201160345.png]]
 ![[Pasted image 20241201161255.png]]
 Много разных [[m_pCurrent]].
 ![[Pasted image 20241201161549.png]]
 Вот тот которые мне нужен в [[packer]]
 ![[Pasted image 20241201162307.png]]
 [[m_aBuffer]].
 ![[Pasted image 20241201162752.png]]
 Есть разные [[m_aBuffer]].
 ![[Pasted image 20241201163222.png]]
 [[Size]] [[m_pCurrent]] [[m_aBuffer]]
 ![[Pasted image 20241201163447.png]]
 Как то разбирусь пойду разбираться в реализации хафмана.
 ![[Pasted image 20241201164032.png]]
 Щас буду разбиратьчя в [[AddInt]] и [[i]]  в ней.
 ![[Pasted image 20241201164449.png]]![[Pasted image 20241201164451.png]]
 Много где юзается и везде как будто не какие то заурядные битовые операции.![[Pasted image 20241201164626.png]]
 Последний шаг это [[m_pEnd]].
 ![[Pasted image 20241201164725.png]]
 [[PACKER_BUFFER_SIZE]] и причем он странно определен ![[Pasted image 20241201164821.png]]
 ![[Pasted image 20241201164942.png]]
 Это в [[Pack]] интовая переменая надо разобраться как работает такое преобразование типов.
 ![[Pasted image 20241201171144.png]]
 Да так я и думал.
 ![[Pasted image 20241201173624.png]]
 вот [[m_pEnd]].
 с ним связан ![[Pasted image 20241201173806.png]]
 [[m_pStart]]. 
 ![[Pasted image 20241201173913.png]]
 [[CUnpacker::Reset]] наверное распоковщик.
 ![[Pasted image 20241201174134.png]]
 давольно понятно в кометариях обьеснено хоть код и так не сложный особенно становится понятно если глянуть как все эти хекс значение выглядят в бин.![[Pasted image 20241204201837.png]]![[Pasted image 20241204201841.png]]
 кажется [[i]] это просто смещение.
 ![[Pasted image 20241204202850.png]]
 Разбираюсь в [[huffman]].
 ![[Pasted image 20241204205738.png]]
 Место где присваевается значение [[m_NumBits]]
 ![[Pasted image 20241205225414.png]]
 как будто это присваивание не имеет смысла.
 ![[Pasted image 20241207145612.png]]
 то есть в верху ему присваивается значение вплоть до 1024 а там наоборот сдвигается назад.
 Bits & 1 тоесть если i четная то ноль а так 1.m_aNodes
 ![[Pasted image 20241207180505.png]]
 как видно depth не может быть больше 32.![[Pasted image 20241207180559.png]]
 вот где юзается алгоритм хафмана то что выше это не сжатие а упоковка.
 Bits | (1 << Depth) тоесть тут прость мы если бит там равен нулю то ноль,иначе 2^Depth-1
 ![[Pasted image 20241208220832.png]]
 Не могу понять смысл алгоритма типо тут скаладываются велечины вон того не понятного массива и сортируются все время пречим сортируется часто массива которая сдвигается в лево.![[Pasted image 20241208230737.png]]
 сыпятся ошибки если пытаюсь скомпилить и всл и в винде
 ![[Pasted image 20241210222318.png]]
 Я понял что единственный смысл [[apNodesLeft]] это 
    m_aNodes[m_NumNodes].m_aLeafs[0] = apNodesLeft[NumNodesLeft - 1]->m_NodeId;

    m_aNodes[m_NumNodes].m_aLeafs[1] = apNodesLeft[NumNodesLeft - 2]->m_NodeId;


тоесть какие то айдишки они меняются при сортировке
![[Pasted image 20241210224100.png]]
есть такая частота которая на порядки больше остальных она будет в любом случае самой большой 536 870 912 как будто тут работает так что чем больше тем реже.![[Pasted image 20241213213334.png]]
![[Pasted image 20241213222018.png]]
вот запись демки![[Pasted image 20241214140915.png]]
![[Pasted image 20241214141149.png]]
![[Pasted image 20241214151023.png]]![[Pasted image 20241214151038.png]]
![[Pasted image 20241214151148.png]]
надо понять от чего чанк тип зависит
![[Pasted image 20241214163432.png]]
![[Pasted image 20241214163932.png]]
![[Pasted image 20241214164142.png]]
![[Pasted image 20241214170012.png]]
![[Pasted image 20241214170600.png]]
![[Pasted image 20241214195351.png]]
![[Pasted image 20241214212230.png]]
![[Pasted image 20241214225551.png]]
![[Pasted image 20241214231859.png]]
![[Pasted image 20241214232441.png]]
![[Pasted image 20241214232918.png]]
![[Pasted image 20241214233138.png]]

![[Pasted image 20241215154753.png]]
кажется вот еще иницилизаторы [[m_aChunkData]]
![[Pasted image 20241215155805.png]]
пытаюсь понять как инитится [[ChunkSize]] с учетом того что он получает данные по сети и связан с демкой.
![[Pasted image 20241215163329.png]]
![[Pasted image 20241215163352.png]]
![[Pasted image 20241215164708.png]]
кажется что ![[Pasted image 20241215164801.png]]
[[WriteTickMarker]] запонлняет хедер.
![[Pasted image 20241215165227.png]]
![[Pasted image 20241215175236.png]]
![[Pasted image 20241215180013.png]]
![[Pasted image 20241215180322.png]]
менюшки демки
![[Pasted image 20241215185413.png]]
![[Pasted image 20241215185440.png]]
![[Pasted image 20241215185459.png]]
[[Slice]] фиг понятно когда вызывается но пока он не нужен.
![[Pasted image 20241215191016.png]]
Из за того что много различных не объединённых в одну условий возможно что [[WriteTickMarker]] не единственный заполняет [[Chunk]].
![[Pasted image 20241215200535.png]]
щас гляну что это за функция вот ее использования:
![[Pasted image 20241215200624.png]]
![[Pasted image 20241215203017.png]]
получается он
![[Pasted image 20241215203511.png]]
инитит все кроме первого(0 индекса) закидывает по 8 бит и [[value]].А [[value]] это [[Tick]].
Очень странно работает.
Опять нашел избыточных код![[Pasted image 20241215204318.png]]
![[Pasted image 20241215204412.png]]
![[Pasted image 20241215204425.png]]
![[Pasted image 20241215213103.png]]
![[Pasted image 20241215220633.png]]
![[Pasted image 20241215221235.png]]
![[Pasted image 20241215221430.png]]
![[Pasted image 20241215221540.png]]![[Pasted image 20241215221540.png]]
тут я остановился [[m_Info]]![[Pasted image 20241215221823.png]]
![[Pasted image 20241215222152.png]]
даже не читая я уверен на 90% что это сранее типо больше 0.7 версии
![[Pasted image 20241215222241.png]]
это чисто по названию то чар максив а то инт и просто упаковывает чары в инт
![[Pasted image 20241216203847.png]]
вспомнил на чем остановился
Вот само структура m_Info
![[Pasted image 20241216204358.png]]
![[Pasted image 20241216204415.png]]
![[Pasted image 20241216212614.png]]
В другой раз разберусь c тиками.
![[Pasted image 20241216221228.png]]
Разбераюсь что такое Crc.
![[Pasted image 20241216221406.png]]
![[Pasted image 20241216221858.png]]
Надо глянуть будет.![[Pasted image 20241216222036.png]]
![[Pasted image 20241216230417.png]]тут остановился
crc в хедере
![[Pasted image 20241217145806.png]]
![[Pasted image 20241217145853.png]]
вот тут можно имя фаила демки получить.
![[Pasted image 20241217152116.png]]
![[Pasted image 20241217152121.png]]
![[Pasted image 20241217152709.png]]
![[Pasted image 20241217160554.png]]
![[Pasted image 20241217160847.png]]
![[Pasted image 20241217201152.png]]
различные состояния клиента.
![[Pasted image 20241217201651.png]]
Получение видюхи.
![[Pasted image 20241217221656.png]]
вот что то с [[Crc]]![[Pasted image 20241217222041.png]]
![[Pasted image 20241217222049.png]]
Это единственное использование [[CalculateHashes]].
![[Pasted image 20241217231603.png]]
Тут остановился 
![[Pasted image 20241217231832.png]]
я так и не понял что в данном контексте  [[Crc]].
![[Pasted image 20241217231948.png]]
Я щас подумал ПОЧЕМУ я не смотрел как плейер работает.
![[Pasted image 20241217232045.png]]
И С этими [[m_aChunkData]] и тд надо продолжить.
![[Pasted image 20241217232211.png]]
![[Pasted image 20241217232337.png]]
наверно в [[Items]] и передается все.
![[Pasted image 20241218194609.png]]
щас гляну возможно это клавиши нажатие.
![[Pasted image 20241218194727.png]]
![[Pasted image 20241218194735.png]]
![[Pasted image 20241218212552.png]]
[[m_File]] он вроде только тут инитится.
![[Pasted image 20241218214800.png]]
[[DemoFile]]
![[Pasted image 20241218214820.png]]
![[Pasted image 20241219223707.png]]
в [[CreateDelta]] и в распаковки дельты надо
![[Pasted image 20241219224241.png]]
единственое место где вызывается
![[Pasted image 20241219224324.png]]
тоже только тут вызывается
![[Pasted image 20241221152827.png]]
![[Pasted image 20241221152831.png]]
![[Pasted image 20241221160451.png]]
![[Pasted image 20241221171227.png]]
я незнаю как не замечал тут даже написано 
![[Pasted image 20241221180505.png]]
может сделать быстрее?
![[Pasted image 20241221180714.png]]
![[Pasted image 20241221190039.png]]
демка на стороне сервера чо?
![[Pasted image 20241221195235.png]]
![[Pasted image 20241221205511.png]]
пытаюсь понять как тут устроено this + что то.
![[Pasted image 20241221205840.png]]
![[Pasted image 20241221205908.png]]
наверное как я и думал и если массив то будет по массиву перемещаться только есть одна не допонятая вещь ![[Pasted image 20241221205946.png]]
почему дата старт с конца итемов?
![[Pasted image 20241222201737.png]]
еще что то не понятное
![[Pasted image 20241222202153.png]]
единственный фади где юзается [[DiffItem]].
![[Pasted image 20241222202341.png]]
я не понимаю это как то связанно с демкой?
![[Pasted image 20241222202530.png]]
![[Pasted image 20241222202621.png]]
я же так и не разобрался что это.
![[Pasted image 20241222210807.png]]
щас увидел этот фаили тут что то с хуком.
![[Pasted image 20241222211012.png]]тут вприцепе с оружием.
![[Pasted image 20241222211055.png]]
![[Pasted image 20241222213218.png]]
тоесть итемом может быть нажатие.
короче тут ![[Pasted image 20241222220353.png]]
тут очень страно идет запись поверх я подозреваю что типо если ты быстро меньше чем тик нажал кнопок то страно не соввсе понятно .
![[Pasted image 20241224235704.png]]
![[Pasted image 20241224235718.png]]
[[aHashlist]].
![[Pasted image 20241224235945.png]]
![[Pasted image 20241225000126.png]]
тут верху функции нулями забиваются и я точно не уверен способен ли компилятор такое оптимизировать и в любом случаи нужны будут лишние теги оптимизации наверное тут нули лучше будут.
Я точно не увередн но генератор хешей создает хэш таблицу.
![[Pasted image 20241225000523.png]]
хотя там [[pTo]] а ниже использование ее [[pFrom]].
![[Pasted image 20241225000808.png]]
тоже не пойму что она делает как будто поиск ну тут линейное время а не O(1).
А кажется тут же поиск в массиве ключей.
![[Pasted image 20241225001055.png]]
![[Pasted image 20241225001557.png]]
Я понял мы получаем ключи из снапшота и потом если несколько раз один и тот же ключ был то m_Num будет больше 1, и будет следующим в массиве ключей, и типа записывается ключ значит он может быть различным то есть это для обработки коллизий при хеширование.
![[Pasted image 20241225002142.png]]
[[m_aIndex]] это номер элемента в снапшоте.
![[Pasted image 20241225232142.png]]
выполняется если в [[pFrom]] нет элемента из [[pTo]].А нет кажется наоборот кажется [[pTo]] более новый снапшот хотя хз пока как это устроено. 
![[Pasted image 20241225233312.png]]
Вот что такое [[дельта]] оо я понял 
![[Pasted image 20241225233348.png]]
дельта в матане это же типо изменение дискретное так вот это и есть изменение тоесть удаленый обьект из снапшота.

![[Pasted image 20241225234419.png]]
о щас [[GetItem]] да как я и думал я предпологал что там типо это перемещние по массиву и вот 
![[Pasted image 20241225235011.png]]
[[pData]] -> const void*.
![[Pasted image 20241225235319.png]]
но я все ровно че то не до понимаю [[DataStart]] 
![[Pasted image 20241226000234.png]]
а че то не совсем понимаю как unsigned char в указатель на снапшот преобразовать.
А кажется я понял это же просто кусок памяти и типо просто класс в него.
![[Pasted image 20241226000916.png]]
хотя тут где присваивание тогда что в [[pData]].
![[Pasted image 20241226003150.png]]
получается все в любом случае уходить вот сюда.
![[Pasted image 20241226225715.png]]
![[Pasted image 20241226232357.png]]
поидеи при ините [[pData]] не может выпонится.
![[Pasted image 20241226232855.png]]
![[Pasted image 20241226234007.png]]
![[Pasted image 20241226234904.png]]
пытаюсь опять разобраться в сжатие.
![[Pasted image 20241227000132.png]]
Я только щас понял что тут в цикле все индексы [[m_NumNodes]] с 257 и при этом увеличивается.
![[Pasted image 20241227001547.png]]
![[Pasted image 20241227001553.png]]
![[Pasted image 20241227002404.png]]
странные название первая разжимает вторая распаковывает.
![[Pasted image 20241227224659.png]]
срабатывает три раза получается.
![[Pasted image 20241228174052.png]]
[[m_aLeafs]] будет ровна 0xffff только тогда когда
![[Pasted image 20241228174156.png]]
[[m_NumNodes]] = HUFFMAN_MAX_SYMBOLS = 257 то есть а вверху наоборот сначала заполняется то есть в низу примерно с 256 по 512 а вверху с 0 до 256 это примерные значения лень точные смотреть. А хотя инитится все просто только спустя 256 будет отрабатывать условия. Я щас задумался о временной сложности этого всего. 
а временая сложность будет 256 и следовательно высота дерева log2_256=8.
![[Pasted image 20241228184350.png]]
тоесть тут вплоть до 256 будет дополнительный бит а нет стоп у нас все время этот сдвиг тоесть в самое левой или правой ветки будет 11111111 и дальше Bits самые первые
![[Pasted image 20241228195543.png]]
а вот обратная операция в [[m_NumBits]] хранится [[Depth]].Хотя нет я щас заметил что тут не приравнивается а прибавляется. А точно я из за непонятного вайла с 0 подумал что цикл и это не обратная операции тут сдвиг в туже сторону
![[Pasted image 20241228204512.png]]
![[Pasted image 20241228211223.png]]
битс же в начале 0 тоесть он равен макс значени 11111111 и 1 или 0 зависит от того "левее он или правее"
![[Pasted image 20241228213602.png]]
биты напрямую переходят в результат
![[Pasted image 20241228220955.png]]
еще одно место
![[Pasted image 20241228221112.png]]
и тут она возрващает сдвиги назад.Тоесть ![[Pasted image 20241228221315.png]]
тут буквально по восем бит записывается
![[Pasted image 20241230225452.png]]
![[Pasted image 20241230225507.png]]
То есть [[m_aChunkData]] юзается сразу в двух а  [[OnDemoPlayerMessage]] это 
![[Pasted image 20241230225826.png]]
![[Pasted image 20241230230020.png]]
![[Pasted image 20241230231335.png]]
тут только инициализируется в этой функции.
Я почему то не обращал внимание что тут тип чанка указывается.
![[Pasted image 20241230231705.png]]
не видел не разу такого типа.
![[Pasted image 20241230232147.png]]
А вот проверки на них.
![[Pasted image 20241230234231.png]]
Я только щас задумался сперва там пакует а потом сжимает как будто наоборот выгоднее.
![[Pasted image 20241230234911.png]]
И да напоминаю первый инты в чары пакует второй уже сжимает.
Я только щас заметил эту связь ![[Pasted image 20241231004646.png]]
![[Pasted image 20241231004703.png]]
![[Pasted image 20241231005019.png]]
меняя это он меняет и это
![[Pasted image 20241231005049.png]]
![[Pasted image 20241231013731.png]]
тут и происходит построение дерева.
![[Pasted image 20241231013822.png]]
и в общем у нас есть начальное распределение мы прибавляем с конца и сортируем  по возрастанию типо 1,2... .
![[Pasted image 20241231014205.png]]
![[Pasted image 20241231014211.png]]
То есть у нас массив указателей.
И меняя распределение мы его меняем и в [[aNodesLeftStorage]] но смысла в этом нет так как оно званого заполнится [[pFrequencies]].
И я понял где ЭТО распределение нужно мы же по ниму 
![[Pasted image 20241231014534.png]]
и сортируем компаратор так сделан.
![[Pasted image 20241231014801.png]]
и мы некогда не затронем только что поменявшеюся частоту. 
![[Pasted image 20241231015219.png]]
Я глянул на таблицу аскии и вспомнил компаратор и понял что да чем выше значение чем чаще ну наверное я сомневаюсь из за огромного [0] элемента 1<<30 это несравнимо больше всех и =>![[Pasted image 20241231015843.png]]
все станут такими большими но если предположить что задом на перед тоесть 10,9,7.....1 например а мы же из за -2 пропускаем первые два.А по факту не важно задом наперд или нет у нас если равномерно примерно тоесть не намного больше следущий а например на 6 то это арифметическая прогрессия и будет квадраты/2 примерно.До сих пор какая то дребедень. 
![[Pasted image 20241231175751.png]]
короче я понял зачем это она делает размер четным тоесть у нас вот как устроенно сжатие и упаковка упаковка просто чары в инты но нам для этого нужно чтоб делилось на 4 что и делает этот кусок кода.
![[Pasted image 20241231181929.png]]
еще из за увеличения размера лишнее заполняет нулями.
![[Pasted image 20241231192703.png]]
![[Pasted image 20241231194735.png]]
![[Pasted image 20241231203731.png]]
![[Pasted image 20241231204612.png]]
я не могу понять у нас размер 512 а мы на 10 спускаемся тоесть как и верху 1024.
![[Pasted image 20250104005953.png]]
![[Pasted image 20250104010004.png]]
Я щас понял что думаю как то очень странно у нас же все далеко не попорядку он перемешивается из за частоты. Тоесть apNodesLeft[NumNodesLeft - 1]->m_NodeId может быть не нулем с учетом того что мы начинаем с конца с нулевых значений так как все перемешалось.
![[Pasted image 20250104231811.png]]
тоесть снапшоты не создаются в [[DoTick]] а наоборот читаются там из фаила.
![[Pasted image 20250105010043.png]]
вроде если не ошибаюсь но там по умолчанию сдивигаются а да я даже жаловался что сложно такое отлаживать
![[Pasted image 20250105010302.png]]
![[Pasted image 20250105151602.png]]
![[Pasted image 20250105222759.png]]
![[Pasted image 20250107165752.png]]
![[Pasted image 20250107165946.png]]
тик легаси на прямую не как не ставится значит он это сочитание
![[Pasted image 20250107232806.png]]
![[Pasted image 20250107234442.png]]
тут остановился
![[Pasted image 20250108165436.png]]
![[Pasted image 20250108171359.png]]
делта это всегда пустое.В дельту записуется только разность точнее измененые итемы
![[Pasted image 20250108175037.png]]
![[Pasted image 20250108192802.png]]
obj возвращается![[Pasted image 20250108192835.png]]
![[Pasted image 20250108195634.png]]
![[Pasted image 20250108200528.png]]
![[Pasted image 20250108200904.png]]
![[Pasted image 20250108203130.png]]
![[Pasted image 20250108221558.png]]
![[Pasted image 20250108221604.png]]
![[Pasted image 20250108223019.png]]
несовмес понятно когда он уходит за uuid![[Pasted image 20250108223241.png]]
![[Pasted image 20250108225046.png]]
Во нужна тут 
![[Pasted image 20250108234043.png]]
![[Pasted image 20250108234051.png]]
![[Pasted image 20250123205353.png]]
![[Pasted image 20250123205533.png]]
![[Pasted image 20250123225254.png]]
![[Pasted image 20250123231131.png]]
![[Pasted image 20250123231157.png]]
![[Pasted image 20250123232107.png]]
то есть это вычисление хэша.
![[Pasted image 20250123232536.png]]
![[Pasted image 20250123232601.png]]
![[Pasted image 20250123232607.png]]
![[Pasted image 20250123233016.png]]
я щас вспомнил что в линуксе его не так давно оптимизировали с помощью маскировки указателей.
![[Pasted image 20250125234317.png]]
![[Pasted image 20250126005836.png]]
![[Pasted image 20250126010939.png]]
![[Pasted image 20250126011624.png]]
![[Pasted image 20250126012552.png]]
позже разбирусь![[Pasted image 20250126014759.png]]
![[Pasted image 20250126015219.png]]
Freq это наверное частота.
![[Pasted image 20250126022014.png]]
![[Pasted image 20250126022027.png]]
![[Pasted image 20250126022157.png]]
![[Pasted image 20250126215506.png]]
![[Pasted image 20250126215636.png]]
![[Pasted image 20250126220628.png]]
![[Pasted image 20250126220654.png]]
![[Pasted image 20250126220948.png]]
![[Pasted image 20250126221539.png]]
![[Pasted image 20250126221841.png]]
![[Pasted image 20250126221856.png]]
![[Pasted image 20250127204858.png]]
![[Pasted image 20250127210308.png]]
![[Pasted image 20250127210319.png]]
![[Pasted image 20250127222947.png]]
![[Pasted image 20250211234313.png]]
вот где используется 
или нет кажется
![[Pasted image 20250212000832.png]]
я щас только обратил внимание что это же плеер и там в экстракт чат тоже плейр тоесть снапшоты это из сохранений.
![[Pasted image 20250212001108.png]]
![[Pasted image 20250212001242.png]]
![[Pasted image 20250212001321.png]]
![[Pasted image 20250212001445.png]]
![[Pasted image 20250212001624.png]]
тоесть если все без ошибкой тоесть там меньше 0 размер и тд то создается снапшот.
![[Pasted image 20250214003752.png]]
![[Pasted image 20250214003848.png]]
![[Pasted image 20250214004045.png]]
![[Pasted image 20250216010940.png]]
читает снапшот![[Pasted image 20250216011005.png]]
![[Pasted image 20250216011043.png]]
![[Pasted image 20250216011443.png]]
![[Pasted image 20250216012102.png]]
а чето забыл почему тут такой алгоритм.А понял 30 и 31 в нижних условиях типо там можно различий размер.Максимальный 2^16 .
![[Pasted image 20250216012517.png]]
вот дельта создается
![[Pasted image 20250216012542.png]]
![[Pasted image 20250216012652.png]]
Тоесть снапшот созадется только в начале записи кажется.
![[Pasted image 20250217233141.png]]
непонимаю откуда такой размер если
![[Pasted image 20250217233223.png]]
![[Pasted image 20250217235518.png]]
тоесть если не срабают условию то почти всегда срабатает то что ниже.
![[Pasted image 20250218001130.png]]
![[Pasted image 20250218004331.png]]
![[Pasted image 20250218220339.png]]
![[Pasted image 20250218222919.png]]
![[Pasted image 20250218223059.png]]
![[Pasted image 20250218223330.png]]
![[Pasted image 20250218223339.png]]
![[Pasted image 20250218223346.png]]
![[Pasted image 20250218223735.png]]
![[Pasted image 20250218231305.png]]
![[Pasted image 20250218231313.png]]
тоесть это постройка дерева.











Вернулся я к этому ~~гиблому~~ делу спустя много време,времени просто небыло особо я не помню задумавался ли что это такое ![[Pasted image 20250608171427.png]]
типо тут запись идет с условиям и щас почекаю его.![[Pasted image 20250608171656.png]]
![[Pasted image 20250608173656.png]]
с этим чуть позже![[Pasted image 20250608173743.png]]
![[Pasted image 20250608182859.png]]
короче он труе почти всегда SetListener по названию понятно устнавляивает значение тому что в if ах.Ну если ![[Pasted image 20250608183953.png]]
все норм и slice(я пока же что он делает но он связан с началом демы и тд)
![[Pasted image 20250608200055.png]]
он тру всегда когда норм со снапшотом все(тоесть нет ошибок с нулевым рамзером и тд) тоесть и тогда старый снапшот
![[Pasted image 20250608200228.png]]
я щас обратил внимание что цикл то бесконечный я хз может нескольско потоков в дднет но может это уже после записи демы.

![[Pasted image 20250609190901.png]]
![[Pasted image 20250609191148.png]]
![[Pasted image 20250609191323.png]]
![[Pasted image 20250609191550.png]]
![[Pasted image 20250610230100.png]]
хуй найдешь без норм поиска 
![[Pasted image 20250615003147.png]]
чтоб собрать надо отключить все связаное с тестами включая их скачивание
Так нет стоп это старая версия а щас чето не работает щас я думаю
щас попытаюсь с системой сборки разобраться
![[Pasted image 20250615005706.png]]
щас почему то не полная сборка.
Я щас вроде бы откатился щас проверю может что то сложнее в системе сборки.
короче попробую новую версию просто другую я хз может чет сломалось.
![[Pasted image 20250615020957.png]]
щас с новым релезом заработала(до этого и та версия работала) 
![[Pasted image 20250615023053.png]]
сука я все поломал ладно завтра уже щас уже 2:30
![[Pasted image 20250615191931.png]]
![[Pasted image 20250615192717.png]]
![[Pasted image 20250615192826.png]]
![[Pasted image 20250615192941.png]]
![[Pasted image 20250615193211.png]]
![[Pasted image 20250615193406.png]]
![[Pasted image 20250615193442.png]]
![[Pasted image 20250615193748.png]]
![[Pasted image 20250615194621.png]]
ладно щас сперва с этими разберусь ![[Pasted image 20250615194852.png]]
![[Pasted image 20250615201920.png]]
![[Pasted image 20250615203359.png]]
![[Pasted image 20250615213751.png]]
![[Pasted image 20250615224528.png]]
вроде это и есть вызов той функции ![[Pasted image 20250615233556.png]]
![[Pasted image 20250615233559.png]]
![[Pasted image 20250616000216.png]]
вот проблемное место типо тут проверка не хочу ли я в wasm скомпить(я не хочу) и типо если нет то нужен атомик а с ним проблемы.Короче завтра решу.![[Pasted image 20250712011651.png]]
![[Pasted image 20250712011703.png]]
![[Pasted image 20250712011710.png]]
![[Pasted image 20250712014818.png]]
![[Pasted image 20250712023106.png]]
![[Pasted image 20250712023138.png]]
вызывается + проверка того что это не какой то кд скачивается а именно скомпилинный![[2025-07-12 02-33-15.mkv]]
![[Pasted image 20250713001202.png]]
![[Pasted image 20250713004502.png]]
![[Pasted image 20250713015303.png]]
![[Pasted image 20250713015332.png]]
![[Pasted image 20250713015648.png]]
![[Pasted image 20250713020114.png]]
![[Pasted image 20250713021413.png]]
![[Pasted image 20250713021455.png]]
![[Pasted image 20250714010848.png]]
страная конструкция не понимаю зачем так делают![[Pasted image 20250714011516.png]]
![[Pasted image 20250714012820.png]]
![[Pasted image 20250714023347.png]]
![[Pasted image 20250714024306.png]]
![[Pasted image 20250714024354.png]]
я так и не понял откуда его значению берутся надо разобраться + конкретно что деалет dotick и в функции по записи Write и они видно чанки и есть типо  delta snap но не понятно полная картина.![[Pasted image 20250716002918.png]]
![[Pasted image 20250716002937.png]]
![[Pasted image 20250716003554.png]]
![[Pasted image 20250716004359.png]]
обьясниение почему так делается.![[Pasted image 20250716010742.png]]
![[Pasted image 20250716010904.png]]
![[Pasted image 20250716011004.png]]
![[Pasted image 20250716013000.png]]
![[Pasted image 20250716024136.png]]
![[Pasted image 20250716031004.png]]
![[Pasted image 20250716213300.png]]
![[Pasted image 20250716213401.png]]
![[Pasted image 20250716214657.png]]
![[Pasted image 20250716214958.png]]
![[Pasted image 20250716215457.png]]
![[Pasted image 20250716215518.png]]
![[Pasted image 20250716220904.png]]
![[Pasted image 20250717005604.png]]
![[Pasted image 20250717010436.png]]
![[Pasted image 20250717024229.png]]
тут остановился
![[Pasted image 20250718175525.png]]
я посмеотрел в любом случае связано с тем что я делаю надо разобраться![[Pasted image 20250718180132.png]]
можно замутить синхронизацию с облаком напрмер + чтоб она работала в другом ядре/потоке процессора и синхронизацию например/возможно через сервер локальной сети
![[Pasted image 20250718190035.png]]
![[Pasted image 20250718190056.png]]
разобраться почему такое условию![[Pasted image 20250718190423.png]]
![[Pasted image 20250718232811.png]]
из за проблем с clangd и microsoft c++ мне приходилось использовать rg(rip grep) я нашел в некоторых ситуация более удобный способ cntr + f.
![[Pasted image 20250718232933.png]]
![[Pasted image 20250718233325.png]]
страно почему не одной функцией сделано , может пофиксить?Также у меня есть очень масштабная идея как улучшеить ddnet , эт добавить динамики например какой то блок на цепи туда сюда вращаюшийся, либо движущийся блоки , тоесть сделать полноценный движок как например в geometry dash что там очень много всего , сделать можно похожие и в ddnet и можно вместо большого количества элементом в редакторе уровня , сделать возможность скриптинга с помощью c например через tinycc.![[Pasted image 20250718234609.png]]
![[Pasted image 20250718234711.png]]
![[Pasted image 20250719001002.png]]
![[Pasted image 20250719001009.png]]
![[Pasted image 20250719001033.png]]
![[Pasted image 20250719001209.png]]
не совсем понимаю зачем его генерировать![[Pasted image 20250719001357.png]]
а точно из за этого то что это хз как понять.![[Pasted image 20250719001518.png]]
![[Pasted image 20250719001533.png]]
![[Pasted image 20250719001640.png]]
![[Pasted image 20250719001850.png]]
![[Pasted image 20250719001923.png]]
![[Pasted image 20250719001932.png]]
и куда оно пишется.![[Pasted image 20250719002119.png]]
![[Pasted image 20250719002733.png]]
![[Pasted image 20250719002806.png]]
![[Pasted image 20250719003332.png]]
желательно вынести всякое оружие и тд например в json фаил чтоб его можно было не то что без перекомпиляции добавлять, а вообще на лету можно даже и код оружия так подгружать например реализовать что то типа abi для взаимодействия.  Я щас задумался а могут ли динамические либы при подключение к проекту использовать его функции чисто теоретически могут а реализации какая надо будет посмотреть. А нет я перепутал для этого нужно буквально встраивание в код, тут нужны статические только как сделать чтоб они взмаимодействовали со встроенным кодом.Хотя не тут больше подходит ipc желательно если важна скорость и латетность это shared memory для обратного взаимодействия. ![[Pasted image 20250719004442.png]]
что это значит
![[Pasted image 20250719004919.png]]
Я короче так подумал, вроде бы у раста очень мощные макросы насчет взаимодействия с ast не знаю но они мощнее чем в с.Этот код можно на него переписать. А для питоне сделать си обертки над tools.![[Pasted image 20250719005407.png]]
вообще для такого функционала лучше всего  wasm он и достаточно быстрый и позволяет быть уверенным что скачав карту не получишь эксплойт, как в браузерах и там писать любой код ![[Pasted image 20250719010146.png]]
![[Pasted image 20250719010238.png]]
и я не знаю пока как понять какая вызывается. ![[Pasted image 20250719010338.png]]
![[Pasted image 20250720214721.png]]
![[Pasted image 20250720233812.png]]вот эту херню тоже надо исправить.![[Pasted image 20250720234025.png]]
![[Pasted image 20250720234204.png]]
![[Pasted image 20250721002455.png]]
![[Pasted image 20250721003121.png]]
![[Pasted image 20250721003133.png]]
![[Pasted image 20250721003156.png]]
![[Pasted image 20250721004126.png]]
![[Pasted image 20250721005012.png]]
![[Pasted image 20250721005410.png]]
![[Pasted image 20250721005602.png]]
это тоже можно сгенерировать типо макросами тут типо m_SpriteHudHammerHitDisabled -> SPRITE_HUD_HAMMER_HIT_DISABLED
надо m_ убрать и от большой до большой буквы все в верх регистр
![[Pasted image 20250721005929.png]]
и тут получается надо просто по списки тоже макросом на изи. И тут вместо сгенерированого кода можно просто чистый раст юзать.![[Pasted image 20250721010754.png]]
тут тоже макросы можно![[Pasted image 20250721010859.png]]
![[Pasted image 20250721011047.png]]
![[Pasted image 20250721013402.png]]
не совсем понятно что он делает
![[Pasted image 20250721015734.png]]
и на серверной и на клиенской части щас почекаю.![[Pasted image 20250721020246.png]]
он как будто больше для функций класс.![[Pasted image 20250721020554.png]]
![[Pasted image 20250721020559.png]]
тут тоже можно генерировать.![[Pasted image 20250721020747.png]]
![[Pasted image 20250723180633.png]]
![[Pasted image 20250723180743.png]]
![[Pasted image 20250723180813.png]]
![[Pasted image 20250723181140.png]]
![[Pasted image 20250723181157.png]]
![[Pasted image 20250723181450.png]]
![[Pasted image 20250723181505.png]]
можно через фаил с метаданными и макросами.![[Pasted image 20250723213037.png]]
![[Pasted image 20250723213523.png]]
![[Pasted image 20250723214254.png]]
![[Pasted image 20250723214430.png]]
![[Pasted image 20250723215123.png]]
![[Pasted image 20250723215143.png]]
почему названия разные.
![[Pasted image 20250724224541.png]]
![[Pasted image 20250724225459.png]]
![[Pasted image 20250724230038.png]]
что это![[Pasted image 20250727165850.png]]
![[Pasted image 20250727181413.png]]
![[Pasted image 20250727182549.png]]
![[Pasted image 20250727183133.png]]
![[Pasted image 20250727183146.png]]
![[Pasted image 20250727183601.png]]
![[Pasted image 20250727183623.png]]
![[Pasted image 20250727184609.png]]
![[Pasted image 20250727191323.png]]
![[Pasted image 20250727192604.png]]
![[Pasted image 20250727192708.png]]
функции именно внутреняя для работы не вызывается на прямую в client/gameclient![[Pasted image 20250727232633.png]]
![[Pasted image 20250727232705.png]]
![[Pasted image 20250727232850.png]]
![[Pasted image 20250727232932.png]]
![[Pasted image 20250727234608.png]]
![[Pasted image 20250727234618.png]]
![[Pasted image 20250727235833.png]]
я не пойму вроде в дднет вся логика должна быть вынесена на сервер но там есть на клиенте что типо логика хука.![[Pasted image 20250728000521.png]]
![[Pasted image 20250728010815.png]]
![[Pasted image 20250728010906.png]]
![[Pasted image 20250728010916.png]]
![[Pasted image 20250728011431.png]]
ВОТ управление
ладно щас не надолго к tinycc вернусь.
![[Pasted image 20250729004944.png]]
![[Pasted image 20250729004950.png]]
![[Pasted image 20250729005020.png]]
![[Pasted image 20250729005644.png]]
![[Pasted image 20250729005738.png]]
![[Pasted image 20250729005925.png]]
![[Pasted image 20250729010045.png]]
![[Pasted image 20250729010206.png]]
![[Pasted image 20250729010223.png]]
![[Pasted image 20250729010306.png]]
![[Pasted image 20250729010625.png]]
я щас понял что в дднет нет оружия хук он работает с любым оружием![[Pasted image 20250729011757.png]]
![[Pasted image 20250729012319.png]]
![[Pasted image 20250729013109.png]]
ВОО![[Pasted image 20250729013202.png]]
![[Pasted image 20250729013240.png]]
ВОО еще![[Pasted image 20250729013315.png]]
щас чекну тест выполняется ли на клиенте это все или эт серверная часть (можно конечно cmake чекнуть но лень + еще ченить чекну напиши херь какуй то с хуками ![[Pasted image 20250729014237.png]]
найти использования тик не могу
![[Pasted image 20250730013800.png]]
немогу не как tick найти
![[Pasted image 20250730014729.png]]![[Pasted image 20250730015023.png]]![[Pasted image 20250730014733.png]]эт серверная часть точно.Короче я хз как такое искать заного кейт поставлю я его удалил только из за того что wsl не было хотя я хз нахуй он мне нужен был.И юзать не vs а clang.
![[Pasted image 20250730020834.png]]
изменения то работают но невозможно тик найти вызов его. Я только щас понял что проблема в lsp server llvm mingw ,  я понял когда и в kate такие же проблемы возникли.  ![[Pasted image 20250730145400.png]]
теперь я понял![[Pasted image 20250730145803.png]]
![[Pasted image 20250730150236.png]]
![[Pasted image 20250730150431.png]]
![[Pasted image 20250730150509.png]]
![[Pasted image 20250730150902.png]]
![[Pasted image 20250730164100.png]]
![[Pasted image 20250730164108.png]]
![[Pasted image 20250730164334.png]]
терь и этот ошибается
![[Pasted image 20250730164600.png]]
![[Pasted image 20250730164627.png]]
![[Pasted image 20250730165327.png]]
![[Pasted image 20250730165348.png]]
Я понял кажется это типо обработка хука правда она![[Pasted image 20250730165413.png]]
происходит только если кто то хукнул![[Pasted image 20250730165447.png]]
это все игроки![[Pasted image 20250730165455.png]]
это тот кого хукнули.![[Pasted image 20250730165625.png]]
кста парсер интуитивный .![[Pasted image 20250730170103.png]]
![[Pasted image 20250731010317.png]]
бля а это уже серверная часть.![[Pasted image 20250731010400.png]]
а нет есть и на клиенте![[Pasted image 20250731010602.png]]
![[Pasted image 20250731010731.png]]
![[Pasted image 20250731011749.png]]
![[Pasted image 20250731012352.png]]
![[Pasted image 20250731012737.png]]
![[Pasted image 20250731012759.png]]
![[Pasted image 20250731013741.png]]
![[Pasted image 20250731230043.png]]
несовсем смысл понятен.![[Pasted image 20250731231325.png]]
мне кажется это тот же тик ![[Pasted image 20250731231345.png]]
![[Pasted image 20250731231548.png]]
ДАДА я так и думал так как названия как будто не такое должно быть типо следуйщий тип а тут не тип,это можно понять по тому что в цикле и так по всем тимах проходится.![[Pasted image 20250731232038.png]]
![[Pasted image 20250731232114.png]]
![[Pasted image 20250731232503.png]]
они с этим тикими связан![[Pasted image 20250731232713.png]]
бля я с клиента на сервер перешёл случайно. 
![[Pasted image 20250731232817.png]]
![[Pasted image 20250731232855.png]]
![[Pasted image 20250731232944.png]]
он все сносит
КСТАТИ: мне кажется это можно арена алокацией оптимизовать раз мы все сносим то типо что не делать целый  цикл а разом их.![[Pasted image 20250731233152.png]]
а этот оптимизируется тем что не по множеству кусков памяти двигаться надо а просто как в стеке указатель смещать. ![[Pasted image 20250731233339.png]]
![[Pasted image 20250801000523.png]]
кажется вот проблема