---
layout: page
parent: lingocity
title: 버디(펫)
description: >
hide_description: true
accent_color: '#ffedb5'
accent_image:
  background: '#303244'
theme_color: '#193747'
sitemap: false
---

<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_anim.gif" style="width:50%" oncontextmenu="return false;">
  </figure>
</div>

<!-- * 목차
{:toc} -->

<details>
<summary class="my-underline" style="margin-bottom:1rem;">관련 소스 코드 일부</summary>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">Pet.cs</summary>
<pre><code class="language-cs">// 각 펫이 가지는 클래스
public class Pet : BaseEntity
{
    public PetFSMController FSMController { get; private set; } // 상태 관리자
    public PetAudioController AudioController { get; private set; } // 소리 관리자

    public PetActionCanvas ActionCanvas { get; private set; } // 노티 UI (말풍선, 레벨 업, 배고픔 표시 등)

    public PetClothesPoint[] ClothesPoints { get; private set; } // 의상 착용 부위
    public Dictionary&lt;PET_CLOTHES_PART, Transform&gt; DefaultClothes { get; private set; } = new(); // 고유 기본 의상 (아무 의상도 착용하지 않았을 때 On)

    private const float TouchAnimCooltime = 3.5f; // 터치 애니메이션 쿨타임
    private float _nextTouchAnimTime = 0f; // 다음 터치 애니메이션 가능 시간

    #region MonoBehaviour 메소드
    private void Awake()
    {
        // 전역 펫 변수 할당
        Managers.GameWorld.Entity.MyPet = this;

        AudioController = transform.GetComponentInChildren&lt;PetAudioController&gt;();
        ActionCanvas = GetComponentInChildren&lt;PetActionCanvas&gt;();

        // 초기화
        InitializeFSMController();
        InitializeClothes();
        GenerateShadow();
    }

    protected override void Start()
    {
        // 캐릭터 옆으로 이동
        WarpNearPlayer(1f, 0f);

        // 노티 UI 끄기
        ActionCanvas.ForceOff();

        // 배고픔 표시 반복 재생 설정
        InvokeRepeating(nameof(ExpressHunger), 180f, 180f);
    }

    private void Update()
    {
        // 현재 상태에 맞는 행동 실행
        FSMController.Update();
    }

    private void OnDestroy()
    {
        // 배고픔 표시 반복 재생 취소
        CancelInvoke(nameof(ExpressHunger));
    }
    #endregion

    #region 기타 메소드
    // 의상 착용 부위 세팅
    private void SetClothesPoints()
    {
        ClothesPoints = new PetClothesPoint[Enum.GetValues(typeof(PET_CLOTHES_PART)).Length];

        for (int i = 0; i < ClothesPoints.Length; i++)
        {
            PET_CLOTHES_PART part = (PET_CLOTHES_PART)i;
            Transform partTransform = transform.FindChildRecursively(part.ToString());

            ClothesPoints[i] = new PetClothesPoint
            {
                Part = part,
                Transform = partTransform,
            };
        }
    }

    // 상호작용 모드 중 터치 시 애니메이션 재생
    public void PlayTouchAnimation()
    {
        if (Time.time < _nextTouchAnimTime)
        {
            return;
        }

        _nextTouchAnimTime = Time.time + TouchAnimCooltime;

        // 유저 데이터 수집용 API
#if !UNITY_EDITOR && UNITY_ANDROID
        Managers.AndroidManager.CallSendXAPI(new Dictionary&lt;string, object&gt;()
        {
            { "pettouch", "Y" },
            { "petinteractiontouch", "N" },
        }, "btn_0001_0012", true);
#endif

        int randomIndex = Random.Range(0, 2);

        Managers.GameWorld.Entity.MyPet.FSMController.ChangeSubState(
            randomIndex == 0 ? PetSubStateAct.Always_Touch_1 : PetSubStateAct.Always_Touch_3);
    }

    ...
}
</code></pre>
</details>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">PetSetter.cs</summary>
<pre><code class="language-cs">// 펫 관련 최상위 클래스
public class PetSetter
{
    public static PetDataHandler DataHandler { get; } = new(); // 펫 관련 데이터 관리자
    public static PetClothesDataHandler ClothesDataHandler { get; } = new(); // 펫 의상 관련 데이터 관리자

    public static UnityAction OnStartPetChangeAction { get; set; } // 펫 변경 요청 전 이벤트
    public static UnityAction&lt;int&gt; OnEndPetChangeAction { get; set; } // 펫 변경 응답 후 이벤트

    // 초기화
    public static async void Initialize()
    {
        // 월드를 모두 불러올 때까지 대기
        await UniTask.WaitUntil(() => Managers.GameWorld.IsWorldLoaded);

        // 보유하고 있는 펫이 없는 경우 예외 처리
        if (DataHandler.OwnedPetData.Count == 0)
        {
            WJLog.LogColor("보유하고 있는 펫이 없음", 3);

            return;
        }

        // 펫 생성
        CreatePet();

        // 이벤트 등록
        OnEndPetChangeAction -= ChangePet;
        OnEndPetChangeAction += ChangePet;
    }

    // 펫 생성
    private static void CreatePet(bool isStepUp = false)
    {
        // 데이터 세팅
        PetData currentPet = DataHandler.CurrentPet;
        string petType = DataContent.Pet.Get(currentPet.DesignId).Type.ToString(); // 현재 펫 종류 (레서팬더, 용, ...)
        string petStepCode = (currentPet.StepGrowth + 1).ToString("D2"); // 현재 펫 단계 (1 ~ 4)

        DataHandler.CurrentPetTypeCode = petType.ToLower();
        DataHandler.CurrentPetStepCode = petStepCode;

        // 오브젝트 생성
        GameObject petObject = Managers.Resource.ReuseInstantiate($"PetPrefab/{petType}/{petType}_Lv{petStepCode}.prefab");
        petObject.name = $"{petType}_{petStepCode}";

        // 펫 진화로 인해 호출된 거라면 바로 진화 완료 상태로 전환
        if (isStepUp)
        {
            Managers.GameWorld.Entity.MyPet.FSMController.ChangeSubState(PetSubStateAct.LevelUp_StepUp_After);
        }
    }

    // 펫 진화
    public static void StepUpPet()
    {
        Managers.GameWorld.Entity.MyPet.FSMController.ClearSubState();

        DestroyPet();

        CreatePet(true);
    }

    // 펫 변경
    public static void ChangePet(int designId)
    {
        Managers.GameWorld.Entity.MyPet.FSMController.ClearSubState();

        DestroyPet();

        CreatePet();
    }

    ...
}
</code></pre>
</details>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">PetDataHandler.cs</summary>
<pre><code class="language-cs">// 펫 관련 데이터 관리 클래스
public class PetDataHandler
{
    public Dictionary&lt;int, PetData&gt; OwnedPetData { get; private set; } = new(); // 보유 중인 펫 데이터들
    public PetData CurrentPet // 현재 소환된 펫 데이터
    {
        get
        {
            var equippedPet = OwnedPetData.FirstOrDefault(petData => petData.Value.IsEquipped);

            return equippedPet.Value;
        }
    }

    public PetStaticData StaticData { get; private set; } = new(); // 펫 관련 정적 데이터들
    public string CurrentPetTypeCode { get; set; } // resser, dragon, ...
    public string CurrentPetStepCode { get; set; } // 01, 02, ...

    // 서버로부터 보유 중인 펫 정보 불러오기
    public void LoadOwnedPetData(ICollection&lt;PetInfo&gt; petInfos)
    {
        foreach (PetInfo petInfo in petInfos)
        {
            // 서버에서 준 펫 정보가 클라이언트에서도 유효한지 체크
            PetModel petModel = DataContent.Pet.Get(petInfo.DesignId);

            if (petModel == null)
            {
                WJLog.LogColor($"포함되지 않은 펫 데이터. ID: {petInfo.DesignId}", 3);

                continue;
            }

            if (OwnedPetData.ContainsKey(petInfo.DesignId))
            {
                WJLog.LogColor($"중복된 펫 데이터. ID: {petInfo.DesignId}", 3);

                continue;
            }

            // 서버에서 관리하는 데이터
            PetData petData = new(petInfo.DataBaseId, petInfo.DesignId, petInfo.PetName,
                petInfo.StepGrowth, petInfo.Level, petInfo.Exp, petInfo.TodayFeedCount, petInfo.IsEquipped, petInfo.AcquireTime, petInfo.ConditionLevel);

            // 클라이언트에서 관리하는 데이터
            petData.Interaction = StaticData.Interactions[petModel.Type];

            // 보유 중인 펫 데이터 Dictionary에 추가
            OwnedPetData.Add(petInfo.DesignId, petData);

            WJLog.LogColor($"펫 불러오기 성공. ID: {petInfo.DesignId}" + (petInfo.IsEquipped ? " (Equipped)" : ""), 3);
        }
    }

    // 현재 소환된 펫에게 줄 수 있는 먹이 목록 얻기
    public List&lt;Item&gt; GetValidFoods()
    {
        List&lt;Item&gt; validFoods = new();

        // 현재 소환된 펫이 먹을 수 있는 먹이 인덱스들
        int[] foodIndexes = DataContent.Pet.Get(CurrentPet.DesignId).FoodIndexes;

        // 해당 먹이들 중 현재 가지고 있는 것들만 추려내기
        foreach (var index in foodIndexes)
        {
            Item food = Managers.Item.GetItem(index);
            int foodCount = Managers.Item.GetItemCount(index);

            if (food != null && foodCount > 0)
            {
                validFoods.Add(food);
            }
        }

        return validFoods;
    }

    // 펫 먹이 주기 후 데이터, 상태 갱신
    public void UpdatePetAfterEat(int foodIndex, int designId, int stepGrowth, int level, int exp, int todayExp)
    {
        if (!OwnedPetData.ContainsKey(designId))
        {
            return;
        }

        // 데이터 갱신
        OwnedPetData[designId].StepGrowth = stepGrowth;
        OwnedPetData[designId].Level = level;
        OwnedPetData[designId].Exp = exp;
        OwnedPetData[designId].TodayFeedCount = todayExp;

        // 상태 갱신
        bool isLevelUp = OwnedPetData[designId].Level != level;
        bool isStepUp = OwnedPetData[designId].StepGrowth != stepGrowth;

        Managers.GameWorld.Entity.MyPet.FSMController.ChangeSubState(PetSubStateAct.Eat_1, isLevelUp, isStepUp, foodIndex);
    }

    // 현재 소환된 펫의 다음 레벨까지 필요한 경험치 얻기
    public int GetRequiredExpForNextLevel()
    {
        if (CurrentPet.Level >= StaticData.MaxLevel)
        {
            return 0;
        }
        else
        {
            return DataContent.PetLevelExp.Get(CurrentPet.Level + 1).RequiredExp;
        }
    }

    ...
}
</code></pre>
</details>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">PetMember.cs</summary>
<pre><code class="language-cs">// 펫 관련 서버 API 요청 및 콜백 관리 클래스
public class PetMember
{
    // 펫 변경 요청
    public static void ChangePet(int designId)
    {
        EquipPetReq request = new()
        {
            PlayerDatabaseId = Managers.DataCollections.Player.id,
            PetDatabaseId = PetSetter.DataHandler.OwnedPetData[designId].DatabaseId
        };

        Managers.Network.EquipPet(request, CallbackChangePet);
    }

    // 펫 변경 요청 콜백
    public static void CallbackChangePet(EquipPetRes response)
    {
        if (response.ErrorCode != ErrorCode.Success)
        {
            WJLog.LogColor("펫 변경 실패. ErrorCode: " + response.ErrorCode, 3);

            return;
        }

        // 데이터 갱신
        int changedPetDesignId = response.EquippedPetInfo.DesignId;
        PetData changedPetData = PetSetter.DataHandler.OwnedPetData[changedPetDesignId];

        foreach (PetData petData in PetSetter.DataHandler.OwnedPetData.Values)
        {
            if (petData == changedPetData)
            {
                petData.IsEquipped = true;

                continue;
            }

            petData.IsEquipped = false;
        }

        // 이벤트 실행
        PetSetter.OnEndPetChangeAction?.Invoke(changedPetDesignId);
    }

    // 펫 의상 구매 요청
    public static void PurchaseClothes(List&lt;int&gt; designIds)
    {
        PurchasePetAvatarReq request = new()
        {
            PlayerDatabaseId = Managers.DataCollections.Player.id,
            PetAvatarDesignIds = designIds
        };

        Managers.Network.PurchasePetAvatar(request, CallbackPurchaseClothes);
    }

    // 펫 의상 구매 요청 콜백
    private static void CallbackPurchaseClothes(PurchasePetAvatarRes response)
    {
        if (response.ErrorCode != ErrorCode.Success)
        {
            WJLog.LogColor("펫 의상 구매 실패. ErrorCode: " + response.ErrorCode, 3);

            // 입고 있는 의상 원래대로
            PetWearingLibrary.ResetWearing();
            WardrobeManager.ControlThumbnailEvent?.Invoke();

            return;
        }

        // 데이터 갱신
        foreach (var infos in response.PurchasePetAvatarInfos)
        {
            PetSetter.ClothesDataHandler.AddOwnedClothesData(
                infos.PetAvatarInfo.DesignId,
                new PetClothesData(
                    infos.PetAvatarInfo.DataBaseId,
                    infos.PetAvatarInfo.DesignId,
                    infos.PetAvatarInfo.PetDataBaseId,
                    infos.PetAvatarInfo.BodyPart,
                    infos.PetAvatarInfo.AcquireTime
                )
            );
        }

        // 재화 갱신
        Managers.DataCollections.GetInventoryInfo.Mony = response.CurrentMony;
        Managers.UI.GetUI&lt;UIMonyAndStar&gt;()?.RewardAttractorPlay(response.UsedMony, 0);

        // 구매한 의상 바로 입기
        PetSetter.ClothesDataHandler.SendClothesEquipData();

        // 연출
        var itemShopPopup = Managers.UI.GetUI&lt;UIItemShopPopup&gt;();
        itemShopPopup?.BlinkClosetText();
    }

    ...
}
</code></pre>
</details>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">PetFSMController.cs</summary>
<pre><code class="language-cs">// 펫 상태 제어 및 관리 클래스
public class PetFSMController
{
    public NavMeshAgent NavMeshAgent { get; private set; }
    public Animator Animator { get; private set; }

    public PetEquipment Equipment { get; private set; } // 펫의 행동에 맞춰 추가되는 용품들 (소품, 효과 등)

    private Dictionary<PetStateType, PetState> _stateDict = new(); // 상태 모음
    private Dictionary<PetSubStateType, PetSubState> _subStateDict = new(); // 하위 상태 모음
    public PetStateType StartStateType { get; private set; }
    public PetStateType PrevStateType { get; private set; }
    public PetStateType CurrentStateType { get; private set; }
    public PetState CurrentState { get; private set; }
    public PetSubState CurrentSubState { get; private set; }

    public PetFSMController(PetStateType state)
    {
        StartStateType = state;
        PrevStateType = state;
        CurrentStateType = state;
    }

    // 상태 진행
    public void Update()
    {
        if (!Managers.GameWorld.IsWorldLoaded)
        {
            return;
        }

        // 현재 하위 상태 우선 진행
        if (CurrentSubState != null)
        {
            CurrentSubState?.Update();

            return;
        }

        // 현재 상태 진행
        CurrentState?.Update();
    }

    // 상태 변경
    public void ChangeState(PetStateType stateType, params object[] args)
    {
        // 현재 상태 종료
        if (CurrentState != null)
        {
            PrevStateType = CurrentState.GetStateType();

            CurrentState.Exit();
        }

        // 새로운 상태 시작
        CurrentStateType = type;
        CurrentState = _stateDict[type];

        CurrentState.Enter(args);
    }

    ...
}
</code></pre>
</details>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">PetState.cs</summary>
<pre><code class="language-cs">// 펫의 각 상태 부모 클래스
public abstract class PetState
{
    protected PetFSMController FSMController { get; private set; } // 상태 관리자

    protected UnityAction OnEnterAction; // 상태 시작 시 이벤트
    protected UnityAction OnExitAction; // 상태 종료 시 이벤트

    public PetState(PetFSMController fsmController)
    {
        FSMController = fsmController;
    }

    public abstract void Enter(params object[] args); // 상태 시작 시
    public abstract void Update(); // 상태 진행 중 (매 프레임 실행)
    public abstract void Exit(); // 상태 종료 시
    public abstract PetStateType GetStateType(); // 상태 타입 반환
}
</code></pre>
</details>

<details style="margin-left:1rem;">
<summary class="my-underline" style="margin-bottom:1rem;">PetSitState.cs</summary>
<pre><code class="language-cs">// 펫 상태 중 하나인 Sit 상태 클래스
public class PetSitState : PetState
{
    public PetSitState(PetFSMController fsmController) : base(fsmController) { }

    // 상태 시작 시
    public override void Enter(params object[] args)
    {
        // 이벤트 등록 및 실행
        OnEnterAction = null;
        OnExitAction = null;

        if (args != null)
        {
            if (args.Length > 0 && args[0] is UnityAction onEnterAction)
            {
                OnEnterAction = onEnterAction;
            }

            if (args.Length > 1 && args[1] is UnityAction onExitAction)
            {
                OnExitAction = onExitAction;
            }
        }

        OnEnterAction?.Invoke();

        // Sit 애니메이션, 효과음 재생
        FSMController.Animator.CrossFadeInFixedTime(PetAnim.Sit_1.ToString(), 0.5f);
        Managers.GameWorld.Entity.MyPet.AudioController.Play(PetAudioClip.Sit_1, true);
    }

    // 상태 진행 중 (매 프레임 실행)
    public override void Update()
    {
        // Sit_idle_1 애니메이션 루프 중 매 첫 프레임에 효과음 재생
        var animatorStateInfo = FSMController.Animator.GetCurrentAnimatorStateInfo(0);

        if (animatorStateInfo.IsName(PetAnim.Sit_Idle_1.ToString()) &&
            Mathf.Approximately(animatorStateInfo.normalizedTime % 1, 0f))
        {
            Managers.GameWorld.Entity.MyPet.AudioController.Play(PetAudioClip.Sit_Idle_1, true);
        }
    }

    // 상태 종료 시
    public override void Exit()
    {
        // 이벤트 실행
        OnExitAction?.Invoke();
        
        // Stand 애니메이션, 효과음 재생
        FSMController.Animator.CrossFadeInFixedTime(PetAnim.Stand_Up_1.ToString(), 0.1f);
        Managers.GameWorld.Entity.MyPet.AudioController.Play(PetAudioClip.Stand_Up_1, true);
    }

    // 상태 타입 반환
    public override PetStateType GetStateType()
    {
        return PetStateType.Sit;
    }
}
</code></pre>
</details>

</details>

<details>
<summary class="my-underline" style="margin-bottom:1rem;">관련 영상</summary>
  <div class="my-media-row">
    <figure>
      <video controls muted style="width:70%;" poster="/assets/img/projects/lingocity/buddy/buddy_interaction_thumbnail.jpg">
        <source src="/assets/img/projects/lingocity/buddy/buddy_interaction_play.mp4" type="video/mp4">
      </video>
      <figcaption>플레이 영상 - 감정 표현</figcaption>
    </figure>
  </div>
</details>


### 소개
{:.my-heading-circle-1}

- 펫 시스템
- 다양한 상호작용
- 링고시티 주요 성장 요소


### 특징
{:.my-heading-circle-1}

- 경험치, 레벨, 단계로 이루어진 성장 시스템
- 4단계 진화
- FSM 패턴 기반 행동
- 전용 아바타

<div style="margin-bottom:2rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_1.png" oncontextmenu="return false;">
    <figcaption>1단계 레서팬더</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_2.png" oncontextmenu="return false;">
    <figcaption>2단계 레서팬더</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_3.png" oncontextmenu="return false;">
    <figcaption>3단계 레서팬더</figcaption>
  </figure>
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_resser_4.png" oncontextmenu="return false;">
    <figcaption>4단계 레서팬더</figcaption>
  </figure>
</div>


### 주요 상호작용
{:.my-heading-circle-1}

- 먹이 주기
- 감정 표현
- 트레이닝
  - 명령어 음성 인식
  - 발화 중심의 링고시티 콘텐츠 컨셉에 맞춘 버디 상호작용

<div style="margin-bottom:2rem;"></div>
<div class="my-media-row">
 <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_training_anim.gif" style="width:80%" oncontextmenu="return false;">
    <figcaption>트레이닝</figcaption>
  </figure>
</div>


### 신규 버디 추가
{:.my-heading-circle-1}

- 링고시티 출시 이후 신규 버디 업데이트 진행
- 버디 전용 아바타 기능 추가

<div style="margin-bottom:2rem;"></div>
<div class="my-media-row">
  <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_dragon_acquisition_anim.gif" style="width:80%" oncontextmenu="return false;">
    <figcaption>신규 버디 획득 연출</figcaption>
  </figure>
</div>
<div class="my-media-row">
 <figure>
    <img src="/assets/img/projects/lingocity/buddy/buddy_avatar.jpg" style="width:70%" oncontextmenu="return false;">
    <figcaption>버디 전용 아바타 보관함</figcaption>
  </figure>
</div>


<!-- 버디는 링고시티에서 유저와 함께하는 펫입니다.<br>
단순한 동행을 넘어 유저와 상호작용하고 다양한 즐거움을 주는 링고시티의 주요 콘텐츠입니다.

버디는 성장 시스템을 가지고 있어 경험치와 레벨을 올리며 점차 진화합니다.<br>
특정 레벨에 도달하면 단계가 상승하고, 이에 따라 외형과 애니메이션이 변화합니다.<br>
먹이를 주거나 특정 명령어를 음성으로 입력해 성장시킬 수 이

 이러한 성장 상태(경험치, 레벨, 단계)는 서버에 저장되어 관리됩니다.<br>
이처럼 버디는 유저와 함께 성장하며, 다양한 모습과 행동으로 게임의 재미를 더합니다.


### 성장 시스템
{:.my-heading-circle-1}

버디는 성장 시스템을 가지고 있습니다.<br>
각 버디에는 레벨이 있으며 특정 레벨에 도달할 때마다 단계가 상승하여 진화하고,<br>
이에 맞게 외형과 애니메이션이 변화합니다.<br>

버디는 먹이를 주거나 특정 명령어를 음성으로 입력해 상호작용하는 방식으로 성장시킬 수 있습니다.<br>
이러한 성장 상태(경험치, 레벨, 단계)는 변화가 생길 때마다 서버에 저장되어 관리됩니다.


### 명령어 음성 인식
{:.my-heading-circle-1}

유저는 버디에게 특정 명령어를 음성으로 입력해 버디와 상호작용할 수 있습니다.<br>
명령어를 올바르게 발화하여 성공하면 버디는 해당 명령에 맞는 행동을 취하고 경험치를 획득합니다.<br>
버디의 레벨이 오를수록 할 수 있는 행동이 늘어나게 됩니다.

발화를 중심으로 하는 링고시티 콘텐츠 컨셉에 맞추어<br>
유저의 발화를 통해 버디와 상호작용하는 것을 목표로 해당 기능을 개발하였습니다.


### FSM
{:.my-heading-circle-1}

버디는 FSM을 적용하여 각 상태를 기반으로 행동하게끔 개발하였습니다.<br>
버디가 가질 수 있는 상태를 구분하였고, 개발 시 버디의 상태 추가, 수정이 용이하도록 설계하고 개발하였습니다.


### 신규 버디 추가
{:.my-heading-circle-1}

버디는 유저들이 가장 좋아하는 콘텐츠 중 하나였고, 이에 맞추어 신규 버디를 추가하기로 결정하었습니다.<br>
버디 획득, 교체 등의 신규 기능 개발과 함께 이후의 확장성을 고려하여 기존의 관련 코드와 구조를 전체적으로 개선하였습니다.


### 아바타
{:.my-heading-circle-1}

신규 버디 출시에 맞추어 버디 전용 아바타 기능도 함께 개발하였습니다.<br>
기존 캐릭터 아바타와는 다르게 버디만의 특수성으로 인해<br>
버디 아바타 시스템에만 별도로 적용할 새로운 정책들과 기능이 필요했고,<br>
개발 과정에서 많은 의논과 테스트를 거쳐 기능 개발을 완료하였습니다. -->