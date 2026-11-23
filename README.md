class DisjointSet{
    public:
    vector<int> parent,rank;
    DisjointSet(int n){
        parent.resize(n+1);
        rank.resize(n+1,0);
        for(int i =0;i<=n;i++){
            parent[i]=i;
        }
    }
    int findpar(int node){
        if(node==parent[node]){
            return node;
        }
        return parent[node]=findpar(parent[node]);
    }
    void union_by_rank(int u,int v){
        int ulp = findpar(u);
        int vlp = findpar(v);
        if(ulp==vlp){
            return;
        }
        if(rank[ulp]<rank[vlp]){
            parent[ulp]=vlp;

        }else if(rank[vlp]<rank[ulp]){
            parent[vlp]=ulp;
        }else{
            parent[vlp]=ulp;
            rank[ulp]++;
        }
    }
};
class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
       int n = accounts.size();
       DisjointSet ds(n);
       unordered_map<string,int> mp;
       for(int i =0;i<n;i++){
           for(int j =1;j<accounts[i].size();j++){//excluding name
           string mail = accounts[i][j];
           if(mp.find(mail)==mp.end()){
               mp[mail]=i;
           }else{
               ds.union_by_rank(i,mp[mail]);
           }

           }
       }
       vector<string> mergedmail[n];
       for(auto it:mp) {
           string mail = it.first;
           int node = ds.findpar(it.second);
           mergedmail[node].push_back(mail);

       }
       vector<vector<string>> ans;
       for(int i =0;i<n;i++){
           if(mergedmail[i].size()==0){
               continue;//for those of the ndoes who does have ultiparent
           }
           sort(mergedmail[i].begin(),mergedmail[i].end());
           vector<string> temp;
           temp.push_back(accounts[i][0]);
           for(auto it:mergedmail[i]){
               temp.push_back(it);
           }
           ans.push_back(temp);
       }
       return ans;
    }
};
