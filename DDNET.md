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
 давольно понятно в кометариях обьеснено хоть код и так не сложный особенно становится понятно если глянуть как все эти хекс значение выглядят в бин.