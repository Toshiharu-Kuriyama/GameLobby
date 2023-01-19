# GameLobby

#include <iostream>
#include <string>

using namespace std;

class Player {
public:
  Player(const string &name = " ");
  string GetName() const;
  Player *GetNext() const;
  void SetNext(Player *next);

private:
  string m_Name;
  // 次のプレイヤ
  Player *m_pNext;
};

Player::Player(const string &name)
    : m_Name(name),
      //ヌルポインタ
      m_pNext(0) {}

string Player::GetName() const 
{
  return m_Name; //この関数が呼ばれたときにm_Nameの値を返す
}

Player *Player::GetNext() const 
{
  return m_pNext; //この関数が呼ばれたときにm_pNextの値を返す
}

void Player::SetNext(Player *next) 
{
  m_pNext = next; // nextの値をm_pNextに受け渡す
}

class Lobby 
{
  friend ostream &operator<<(ostream &os, const Lobby &aLobby);

public:
  Lobby();
  ~Lobby();
  void AddPlayer();
  void RemovePlayer();
  void Clear();

private:
  Player *m_pHead;
};

Lobby::Lobby()
    : //ヌルポインタ
      m_pHead(0) {}

Lobby::~Lobby() { Clear(); }

void Lobby::AddPlayer() 
{
  //新しいプレイヤノードを作成
  cout << "Please enter a name of the new player: ";
  string name;
  cin >> name;
  Player *pNewPlayer = new Player(name);

  //リストは空っぽの状態だと、新しいプレイヤがリストの最初の項目となる
  // if list is empty, make head of list this new player
  if (m_pHead == 0) 
  {
    m_pHead = pNewPlayer;
  }
  //　ではなければリストの最終項目に追加する
  // otherwise find the end of the list and add the player there
  else 
  {
    Player *pIter = m_pHead;
    while (pIter->GetNext() != 0) 
    {
      pIter = pIter->GetNext();
    }
    pIter->SetNext(pNewPlayer);
  }
}

void Lobby::RemovePlayer() 
{

  if (m_pHead == 0) // m_pHeadにプレイヤーが格納されていない場合
  {
    //ゲームロビーには誰もいません。削除することができません。
    cout << "The game lobby is empty.	No one to remove!\n";
  } 
  else // m_pHeadにプレイヤーが格納されている場合
  {
    // pTempに最初の項目の名前を格納して、m_pHeadには次の項目の名前を格納する
    Player *pTemp = m_pHead;
    m_pHead = m_pHead->GetNext();
    // pTempのメモリを開放する
    delete pTemp;
  }
}

void Lobby::Clear() 
{
  // m_pHeadの要素数が0になるまでRemovePlayer()を呼び出し続ける
  while (m_pHead != 0) 
  {
    RemovePlayer();
  }
}

ostream &operator<<(ostream &os, const Lobby &aLobby) 
{
  Player *pIter = aLobby.m_pHead;
  //現在のゲームロビーにいるプレイヤリスト：
  os << "\nHere's who's in the game lobby:\n";
  if (pIter == 0) 
  {
    //ゲームロビーには誰もいません。
    os << "The lobby is empty.\n";
  } 
  else 
  {
    //先頭から順番にプレイヤーをGetName()に受け渡していく
    while (pIter != 0) 
    {
      os << pIter->GetName() << endl;
      pIter = pIter->GetNext();
    }
  }

  return os;
}

int main() {
  Lobby myLobby;
  int choice;

  do 
  {
    cout << myLobby;
    //ゲームロビー
    cout << "\nGAME LOBBY\n";
    //プログラムを終了
    cout << "0 - Exit the program\n";
    //ゲームロビーにプレイヤを追加
    cout << "1 - Add a player to the lobby\n";
    //ゲームロビーからプレイヤを削除
    cout << "2 - Remove a player from the lobby\n";
    //ゲームロビーをクリアする
    cout << "3 - Clear the lobby\n";
    //選択を入力
    cout << endl << "Enter a choice: ";
    cin >> choice;

    switch (choice) 
    {
    //さようなら
    case 0:
      cout << "Good-bye\n";
      break;
    case 1:
      myLobby.AddPlayer();
      break;
    case 2:
      myLobby.RemovePlayer();
      break;
    case 3:
      myLobby.Clear();
      break;
      //不正入力をしました
    default:
      cout << "That was not a valid choice\n";
    }
  } while (choice != 0);

  return 0;
}