# Prefix Tree
## data structure implementation
```java
class Trie {
    class TrieNode{
        char val;
        TrieNode[] children;
        boolean isWord;
        TrieNode(char c){
            this.val=c;
            this.children=new TrieNode[26];
            this.isWord=false;
        }
    }
    TrieNode root;
    /** Initialize your data structure here. */
    public Trie() {
        root=new TrieNode('#');
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode tmp=root;
        for(char c:word.toCharArray()){
            if(tmp.children[c-'a']==null){
                tmp.children[c-'a']=new TrieNode(c);
            }
            tmp=tmp.children[c-'a'];
        }
        tmp.isWord=true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode tmp=root;
        for(char c:word.toCharArray()){
            if(tmp.children[c-'a']==null){
                return false;
            }
            tmp=tmp.children[c-'a'];
        }
        return tmp.isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode tmp=root;
        for(char c:prefix.toCharArray()){
            if(tmp.children[c-'a']==null){
                return false;
            }
            tmp=tmp.children[c-'a'];
        }
        return true;
    }
}
```
## Time Complexity
The complexity of creating a trie is O(W*L), where W is the number of words, and L is an average length of the word: you need to perform L lookups on the average for each of the W words in the set.Same goes for looking up words later: you perform L steps for each of the W words.Hash insertions and lookups have the same complexity: for each word you need to check equality, which takes O(L), for the overall complexity of O(W*L).

If you need to look up entire words, hash table is easier. However, you cannot look up words by their prefix using a hash table; If prefix-based lookups are of no interest to you, use a hash table; otherwise, use a trie.
 
