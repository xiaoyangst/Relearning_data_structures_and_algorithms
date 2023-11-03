```c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> get_next(string pattern) {
    if (pattern.empty()) {
        return {};
    }
    int n = pattern.size();
    vector<int> next(n, 0);
    int j = 1, k = 0;
    while (j < n) {
        if (pattern[j] == pattern[k]) {
            next[j] = k + 1;
            j++;
            k++;
        } else {
            if (k > 0) {
                k = next[k - 1];
            } else {
                next[j] = 0;
                j++;
            }
        }
    }
    return next;
}

vector<int> kmp_search(string text, string pattern) {
    vector<int> result;
    int m = text.size();
    int n = pattern.size();
    if (n == 0) {
        return result;
    }
    
    vector<int> next = get_next(pattern);
    int j = 0, k = 0; 
    while (j < m) { 
        if (text[j] == pattern[k]) { 
            j++;
            k++; 
            if (k == n) {
                result.push_back(j - k);
                k = next[k - 1];
            }
        } else {
            if (k > 0) {
                k = next[k - 1];
            } else {
                j++; 
            }
        }
    }
    return result;
}

int main() {
    string text = "ABABDABACDABABCABAB";
    string pattern = "ABABCABAB";
    
    vector<int> match_positions = kmp_search(text, pattern);
    
    if (match_positions.empty()) {
        cout << "Pattern not found in the text." << endl;
    } else {
        cout << "Pattern found at positions: ";
        for (int pos : match_positions) {
            cout << pos << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

