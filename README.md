#include <iostream>
#include <map>
#include <thread>

using namespace std;

mutex mymutex;

void thread1(int size, map<string,string>* inmap)
{
    while (1)
    {
        if (inmap->size()>size)
        {
            mymutex.lock();
            for (auto i = inmap->begin(); i != inmap->end(); ++i)
            {
                cout << (i->first) << " " << (i->second) << endl;
            }
            size++;
            mymutex.unlock();
        }
    }
    
}


int main()
{
  string readline, readdata;
  int mapsize = 0;
  map <string, string> mymap;
  thread t = thread(thread1 , ref(mapsize), &mymap);
  t.detach();
    
  while (cout<<"Введите фамилию или \"выход\" для выхода.\n" && getline(cin,readline) && readline != "выход")
  {
    cout<<"Введите имя и отчество:"<<endl;
    getline(cin, readdata);
    mymap.insert(pair<string,string>(readline,readdata));
  }
}
