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
