# Trie

* Trie is used to search words
* TrieNode structure

```cpp
struct TrieNode {
        TrieNode* child[26];
        bool isWord; // or store "string word;"
        TrieNode() : isWord(false) {
            for (auto& c : child) { // must use & here to mutate child
                c = NULL;
            }
        }
    };
```

* Basic functions

```cpp
TrieNode* root;

/** Initialize your data structure here. */
Trie() {
    root = new TrieNode();
}

/** Inserts a word into the trie. */
void insert(string word) {
    TrieNode* cur = root;
    for (auto ch : word) {
        int idx = ch - 'a';
        if (!cur->child[idx]) {
            cur->child[idx] = new TrieNode();
        }
        cur = cur->child[idx];
    }
    cur->isWord = true;
}

/** Returns if the word is in the trie. */
bool search(string word) {
    TrieNode* cur = root;
    for (auto ch : word) {
        int idx = ch - 'a';
        if (!cur->child[idx]) {
            return false;
        }
        cur = cur->child[idx];
    }
    return cur->isWord;
}

/** Returns if there is any word in the trie that starts with the given prefix. */
bool startsWith(string prefix) {
    TrieNode* cur = root;
    for (auto ch : prefix) {
        int i = ch - 'a';
        if (!cur->child[i]) {
            return false;
        }
        cur = cur->child[i];
    }
    return true;
}
```

