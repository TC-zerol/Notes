1.给一个n（<=10），按字典序输出1~n的排列。（要求排列中相邻数字绝对值不为1）

输入：

4
输出：
2 4 1 3
3 1 4 2

```java
class Solution {
    ArrayList<ArrayList<Integer>> ana = new ArrayList<>();
    int[] vis =new int[11];
    int n;
    public void no1() {

        Scanner input = new Scanner(System.in);
        n = input.nextInt();
        for (int i = 1; i <= n; i++) {
            ArrayList<Integer> a = new ArrayList<>();
            dfs(i,a);
        }
        for(ArrayList<Integer> out : ana){
            for(int outI : out){
                System.out.print(outI);
                System.out.print(" ");
            }
            System.out.println("");
        }
    }
    private void dfs(int x,ArrayList<Integer> a){
        vis[x] = 1;
        a.add(x);
        if(a.size() == n){
            vis[x] = 0;
            ana.add(a);
            return;
        }
        for (int i = 1; i <= n; i++) {
            if (Math.abs(x - i) != 1 && vis[i] == 0){
                dfs(i,(ArrayList<Integer>)a.clone());
            }
        }
        vis[x] = 0;
    }
```

