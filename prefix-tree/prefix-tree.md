# Prefix Tree
data structure implementation
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

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
 ```
