Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

Example:
Given a / b = 2.0, b / c = 3.0. 
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? . 
return [6.0, 0.5, -1.0, 1.0, -1.0 ].

The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.

According to the example above:
```
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```
The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.

__My Idea__
I'm not quite familiar with graph-related problem before. But, it is not difficult to see that finding the answer of querie is a searching process. All given equation could construct a graph, in which nodes are the variables, the edges are equation, and weights are those double variables. Two questions come up with the problem when I seeing. First is use what data structure to construct the graph. I intuitively feel that HashMap might be deployed, because it stores the tuples. What is the best way to use HashMap, and could it be the solution with best complexity? The second question is how to search a graph. I know how to search a tree, but is it same thing? I make sure about that. 
So much things are unfamiliar makes me hesitate. I can't make sure that whether directly searching would cause TLE. After glance the TOP solution, it proves that my worries are unnecessary. 

So, for me, the practices that come with this question are following things:
1. HashMap using 
2. Graph searching 

__My Soltuion__
```
HashMap<String, ArrayList<String>> edges = new HashMap<>();
    HashMap<String, ArrayList<Double>> weights = new HashMap<>();
    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        double[] ret = new double[queries.length];
        for(int i = 0; i < equations.length; i++)
        {
            String[] paris = equations[i];
            ArrayList<String> edge   = (ArrayList)edges.get(paris[0]);
            ArrayList<Double> weight = (ArrayList)weights.get(paris[0]);
            if(edge == null)
            {
                edge = new ArrayList();
                edges.put(paris[0], edge);
            }
            if(weight == null)
            {
                weight = new ArrayList();
                weights.put(paris[0], weight);
            }
            edge.add(paris[1]);
            weight.add(values[i]);
            
            edge   = (ArrayList)edges.get(paris[1]);
            weight = (ArrayList)weights.get(paris[1]);
            if(edge == null)
            {
                edge = new ArrayList();
                edges.put(paris[1], edge);
            }
            if(weight == null)
            {
                weight = new ArrayList();
                weights.put(paris[1], weight);
            }
            edge.add(paris[0]);
            weight.add((double)(1 / values[i]));
        }
        
        for(int i = 0; i < queries.length; i++)
        {
            ret[i] = DFS(queries[i][0], queries[i][1], new HashSet<String>());
        }
        return ret;
    }
    public double DFS(String x, String y, HashSet<String> status)
    {
        double ret = -1.0;
        if(status.contains(x))
            return ret;
        List<String> edge   = (ArrayList)edges.get(x);
        List<Double> weight = (ArrayList)weights.get(x);
        if(edge == null)
            return ret;
        
        if(edge.contains(y))
        {
            int index = edge.indexOf(y);
            return weight.get(index);
        }

        for(int i = 0; i < edge.size(); i ++)
        {
            status.add(x);
            ret = DFS(edge.get(i), y, status);
            status.remove(x);
            if(ret != -1.0)
                return ret * weight.get(i);
        }
        return ret;
    }
```
