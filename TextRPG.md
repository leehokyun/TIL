using System;
using System.Collections.Generic;

// 캐릭터 정보 클래스
class Character
{
    public int Level { get; set; } = 1;
    public string Name { get; set; } = "Chad";
    public string Job { get; set; } = "전사";
    public int AttackPower { get; set; } = 10;
    public int DefensePower { get; set; } = 5;
    public int Health { get; set; } = 100;
    public int Gold { get; set; } = 1500;
}

// 아이템 클래스
class Item
{
    public string Name { get; set; }
    public int AttackBonus { get; set; }
    public int DefenseBonus { get; set; }
    public string Description { get; set; }
    public int Price { get; set; }
    public bool IsEquipped { get; set; }
    public bool IsPurchased { get; set; }  // 구매 여부 추가

    public Item(string name, int attackBonus, int defenseBonus, string description, int price = 0)
    {
        Name = name;
        AttackBonus = attackBonus;
        DefenseBonus = defenseBonus;
        Description = description;
        Price = price;
        IsEquipped = false;
        IsPurchased = false;
    }

    // 장착 상태에 따른 이름 반환
    public string GetDisplayName()
    {
        return IsEquipped ? $"[E]{Name}" : Name;
    }

    // 상점에서 구매 여부에 따른 이름 반환
    public string GetShopDisplayName()
    {
        return IsPurchased ? $"{Name} | 구매완료" : $"{Name} | {Price} G";
    }
}

class Program
{
    static Character player = new Character();

    // 인벤토리 리스트
    static List<Item> inventory = new List<Item>
    {

    }; //초기에 인벤토리 비어있음

    // 상점에서 판매할 아이템 리스트
    static List<Item> shopItems = new List<Item>
    {
        new Item("수련자 갑옷", 0, 5, "수련에 도움을 주는 갑옷입니다.", 1000),
        new Item("무쇠갑옷", 0, 9, "무쇠로 만들어져 튼튼한 갑옷입니다.", 100),
        new Item("스파르타의 갑옷", 0, 15, "스파르타의 전사들이 사용했다는 전설의 갑옷입니다.", 3500),
        new Item("낡은 검", 2, 0, "쉽게 볼 수 있는 낡은 검 입니다.", 600),
        new Item("청동 도끼", 5, 0, "어디선가 사용됐던거 같은 도끼입니다.", 1500),
        new Item("스파르타의 창", 7, 0, "스파르타의 전사들이 사용했다는 전설의 창입니다.", 100)
    };

    static void Main(string[] args)
    {
        while (true)
        {
            ShowMainMenu();
        }
    }

    // 메인 메뉴
    static void ShowMainMenu()
    {
        Console.Clear();
        Console.WriteLine("스파르타 마을에 오신 여러분 환영합니다.");
        Console.WriteLine("이곳에서 던전으로 들어가기전 활동을 할 수 있습니다.");
        Console.WriteLine("1. 상태 보기");
        Console.WriteLine("2. 인벤토리");
        Console.WriteLine("3. 상점");
        Console.Write("원하시는 행동을 입력해주세요.\n>> ");

        string input = Console.ReadLine();
        switch (input)
        {
            case "1":
                ShowStatus();
                break;
            case "2":
                ShowInventory();
                break;
            case "3":
                ShowShop();
                break;
            default:
                Console.WriteLine("잘못된 입력입니다.");
                Console.ReadLine();
                break;
        }
    }

    // 상태 보기 화면
    static void ShowStatus()
    {
        Console.Clear();
        Console.WriteLine("캐릭터의 정보가 표시됩니다.");
        Console.WriteLine($"Lv. {player.Level}");
        Console.WriteLine($"{player.Name} ( {player.Job} )");
        Console.WriteLine($"공격력 : {player.AttackPower}");
        Console.WriteLine($"방어력 : {player.DefensePower}");
        Console.WriteLine($"체 력 : {player.Health}");
        Console.WriteLine($"Gold : {player.Gold} G");
        Console.WriteLine("\n0. 나가기");
        Console.Write("원하시는 행동을 입력해주세요.\n>> ");
        Console.ReadLine();
    }

    // 인벤토리 화면
    static void ShowInventory()
    {
        Console.Clear();
        Console.WriteLine("인벤토리");
        Console.WriteLine("보유 중인 아이템을 관리할 수 있습니다.");
        Console.WriteLine("\n[아이템 목록]");
        foreach (var item in inventory)
        {
            Console.WriteLine($"- {item.GetDisplayName()} | 공격력 +{item.AttackBonus} | 방어력 +{item.DefenseBonus} | {item.Description}");
        }
        Console.WriteLine("\n1. 장착 관리");
        Console.WriteLine("2. 나가기");
        Console.Write("원하시는 행동을 입력해주세요.\n>> ");
        string input = Console.ReadLine();

        switch (input)
        {
            case "1":
                ManageEquip();
                break;
            case "2":
                break;
            default:
                Console.WriteLine("잘못된 입력입니다.");
                Console.ReadLine();
                break;
        }
    }

    // 장착 관리 화면
    static void ManageEquip()
    {
        Console.Clear();
        Console.WriteLine("인벤토리 - 장착 관리");
        Console.WriteLine("보유 중인 아이템을 관리할 수 있습니다.\n");
        Console.WriteLine("[아이템 목록]");
        for (int i = 0; i < inventory.Count; i++)
        {
            Console.WriteLine($"- {i + 1} {inventory[i].GetDisplayName()} | 공격력 +{inventory[i].AttackBonus} | 방어력 +{inventory[i].DefenseBonus} | {inventory[i].Description}");
        }
        Console.WriteLine("\n0. 나가기");
        Console.Write("원하시는 행동을 입력해주세요.\n>> ");
        string input = Console.ReadLine();

        if (int.TryParse(input, out int itemNumber) && itemNumber >= 1 && itemNumber <= inventory.Count)
        {
            ToggleEquip(itemNumber - 1);
        }
        else if (input != "0")
        {
            Console.WriteLine("잘못된 입력입니다.");
            Console.ReadLine();
        }
    }

    // 아이템 장착/해제 토글
    static void ToggleEquip(int index)
    {
        var item = inventory[index];
        item.IsEquipped = !item.IsEquipped;

        if (item.IsEquipped)
        {
            player.AttackPower += item.AttackBonus;
            player.DefensePower += item.DefenseBonus;
        }
        else
        {
            player.AttackPower -= item.AttackBonus;
            player.DefensePower -= item.DefenseBonus;
        }

        Console.WriteLine($"{item.Name}이(가) {(item.IsEquipped ? "장착" : "해제")}되었습니다.");
        Console.ReadLine();
    }

    // 상점 화면
    static void ShowShop()
    {
        Console.Clear();
        Console.WriteLine("상점");
        Console.WriteLine("필요한 아이템을 얻을 수 있는 상점입니다.");
        Console.WriteLine($"\n[보유 골드]\n{player.Gold} G");
        Console.WriteLine("\n[아이템 목록]");
        foreach (var item in shopItems)
        {
            Console.WriteLine($"- {item.GetShopDisplayName()} | 공격력 +{item.AttackBonus} | 방어력 +{item.DefenseBonus} | {item.Description}");
        }
        Console.WriteLine("\n1. 아이템 구매");
        Console.WriteLine("0. 나가기");
        Console.Write("원하시는 행동을 입력해주세요.\n>> ");
        string input = Console.ReadLine();

        switch (input)
        {
            case "1":
                BuyItem();
                break;
            case "0":
                return;
            default:
                Console.WriteLine("잘못된 입력입니다.");
                Console.ReadLine();
                break;
        }
    }

    // 아이템 구매 관리 화면
    static void BuyItem()
    {
        Console.Clear();
        Console.WriteLine("상점 - 아이템 구매");
        Console.WriteLine("필요한 아이템을 얻을 수 있는 상점입니다.");
        Console.WriteLine($"\n[보유 골드]\n{player.Gold} G");
        Console.WriteLine("\n[아이템 목록]");
        for (int i = 0; i < shopItems.Count; i++)
        {
            Console.WriteLine($"- {i + 1} {shopItems[i].GetShopDisplayName()} | 공격력 +{shopItems[i].AttackBonus} | 방어력 +{shopItems[i].DefenseBonus} | {shopItems[i].Description}");
        }
        Console.WriteLine("\n0. 나가기");
        Console.Write("원하시는 행동을 입력해주세요.\n>> ");
        string input = Console.ReadLine();

        if (int.TryParse(input, out int itemNumber) && itemNumber >= 1 && itemNumber <= shopItems.Count)
        {
            ProcessPurchase(itemNumber - 1);
        }
        else if (input != "0")
        {
            Console.WriteLine("잘못된 입력입니다.");
            Console.ReadLine();
        }
    }

    // 구매 처리 함수
    static void ProcessPurchase(int index)
    {
        var item = shopItems[index];

        if (item.IsPurchased)
        {
            Console.WriteLine("이미 구매한 아이템입니다.");
            Console.ReadLine();
            return;
        }

        if (player.Gold >= item.Price)
        {
            player.Gold -= item.Price;
            item.IsPurchased = true;
            inventory.Add(item);
            Console.WriteLine($"{item.Name}을(를) 구매했습니다.");
            Console.WriteLine($"남은 골드: {player.Gold}");
        }
        else
        {
            Console.WriteLine("Gold 가 부족합니다.");
        }
        Console.ReadLine();
    }
}
