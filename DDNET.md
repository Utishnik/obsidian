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
