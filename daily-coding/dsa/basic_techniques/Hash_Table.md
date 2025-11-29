# Hash table
## Cài đặt Seperate Chaining
```cpp
const int cap = 1e6 + 3;
vector<pair<int, int>> bucket[cap]; // mảng 2 chiều pair
struct hashTable{
    int function(int key){
        return key % cap;
    }
    void insert(int key, int val){
        int k = function(key);
        bucket[k].push_back({key, val});
    }
    bool find(int key, int val){
        int k = function(key);
        for(auto x : bucket[k]){
            if(x.first == key && x.second == val) return true;
        }
        return false;
    }
    void get(int key){
        int k = function(key);
        for(auto x : bucket[k]){
            if(x.first == key) cout << x.second << ' ';
        }
        cout << endl;
    }
    void remove(int key, int val){
        int k = function(key);
        for(int i = 0; i < bucket[k].size(); i++){
            if(bucket[k][i].first == key && bucket[k][i].second == val){
                bucket[k].erase(bucket[k].begin() + i);
                return;
            }
        }
    }
    void removeAll(int key){
        int k = function(key);
        bucket[k].clear();
        return;
    }
    void print(int n){
        for(int i = 0; i < n; i++){
            cout << i << " : " << '[' << bucket[i][0].first << " : ";
            for(int j = 0; j < bucket[i].size(); j++){
                if(j == bucket[i].size() - 1) cout << bucket[i][j].second;
                else cout << bucket[i][j].second << ", ";
            }
            cout << ']' << endl;
        }
    }
};

int main(){
    //thử nghiệm
    int a[10] = { 12, 3, 23, 4, 11, 32, 26, 33, 17, 19 };
    hashTable h;
    for(int i = 0; i < 10; i++){
        h.insert(i, a[i]);
    }
    h.insert(2, 89);
    h.insert(4, 78);
    h.removeAll(4);
    h.get(2);
    cout << h.find(7, 17) << endl;
    h.print(10);
}
```
## Cài đặt Open addressing
```cpp
const int cap = 1e6 + 3;
vector<pair<int, int>> bucket(cap, {-1, -1}); //mảng pair
int cnt;
struct hashTable{
    int function(int key){
        return key % cap;
    }
    void insert(int key, int val){
        int k = function(key);
        while(bucket[k].first != -1 && bucket[k].first == key){
            k = (k + 1) % cap;
        }
        bucket[k] = {key, val};
        cnt = max(cnt, k);
    }
    int find(int key, int val){
        int k = function(key);
        while(bucket[k].second != val){
            k = (k + 1) % cap;
        }
        return k;
    }
    void remove(int key, int val){
        int i = find(key, val);
        bucket[i] = {-1, -1};
    }
    void print(){
        for(int i = 0; i <= cnt; i++){
            if(bucket[i].first != -1 && bucket[i].second != -1){
                cout << i << " : "<< "[" << bucket[i].first << ", " << bucket[i].second << "]" << endl;
            }
        }
    }

};

int main(){
    //thử nghiệm
    int a[10] = { 12, 3, 23, 4, 11, 32, 26, 33, 17, 19 };
    hashTable h;
    for(int i = 0; i < 10; i++){
        h.insert(i, a[i]);
    }
    h.insert(2, 89);
    h.insert(4, 78);
    h.remove(4, 78);
    cout << h.find(2, 89) << endl;
    h.print();
    
}
```