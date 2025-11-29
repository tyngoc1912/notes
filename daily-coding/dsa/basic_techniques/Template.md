# Template C++
```cpp
#include <bits/stdc++.h>
using namespace std;

#define ms(s, n) memset(s, n, sizeof(s))
#define all(a) a.begin(), a.end()
#define sz(a) int((a).size())
#define present(t, x) (t.find(x) != t.end())
#define FOR(i, a, b) for(int i = (a); i <= (b); i++)
#define FORd(i, a, b) for(int i = (a) - 1; i >= b; i--)
#define pb push_back
#define pf push_front
#define fi first
#define se second
#define mp make_pair
#define endl "\n"

typedef long long ll;
typedef unsigned long long ull;
typedef long double ld;
typedef pair<int, int> pi;
typedef vector<int> vi;
typedef vector<pi> vii;

const int MOD = (int) 1e9 + 7;
const int INF = (int) 1e9 + 2804;
const int MAXN = (int) 1e5 + 105;
inline ll gcd(ll a, ll b){ll r; while(b){r=a%b; a=b; b=r;} return a;}
inline ll lcm(ll a, ll b){return a/gcd(a,b)*b;}

int main(){
    #ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    #endif 
    ios::sync_with_stdio(false); 
    cin.tie(nullptr); cout.tie(nullptr);
    
}
```