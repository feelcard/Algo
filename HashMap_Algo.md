```java
package com.asdf;

import java.util.*;

class Solution_HashMap {
    public int[] solution(String[] genres, int[] plays) {

        PriorityQueue<MusicList> gList;
        //MusicList 객체를 저장하는 우선수위 큐 선언
        HashMap<String, MusicList> tmpHM = new HashMap<>();
        //장르를 key값으로 갖고 MusicList객체를 value로 갖는 HashMap 선언 및 생성
        for (int i = 0; i < genres.length; i++) {
            Music m = new Music(i, plays[i]);
            //음원의 고유번호와 재생횟수를 저장한 Music 객체
            if(tmpHM.containsKey(genres[i])) 
                //map에 있는 데이터 중 기존에 있는 genres가 있으면 
                tmpHM.get(genres[i]).add(m);
            	//MusicList 에 Music을 추가
            else 
                tmpHM.put(genres[i], new MusicList(m));
        }
        gList = new PriorityQueue<>(tmpHM.values());
        ArrayList<Integer> tmpAns = new ArrayList<>();
        while(!gList.isEmpty()) {
            MusicList tmp = gList.poll();
            tmpAns.add(tmp.mPQ.poll().id);
            if(tmp.mPQ.size() != 0)
                tmpAns.add(tmp.mPQ.poll().id);
        }
        int[] answer = new int[tmpAns.size()];
        Iterator<Integer> it = tmpAns.iterator();
        int i=0;
        while(it.hasNext()) {
            answer[i++] = it.next();
        }
        return answer;
    }
}
class MusicList implements Comparable<MusicList>{
    int total=0;
    PriorityQueue<Music> mPQ = new PriorityQueue<>();
    public MusicList(Music m) {
        this.total += m.plays;
        mPQ.add(m);
    }
    public void add(Music m) {
        this.total += m.plays;
        mPQ.add(m);
        if(mPQ.size()>2) {
            PriorityQueue<Music> tmp = new PriorityQueue<>();
            tmp.add(mPQ.poll());
            tmp.add(mPQ.poll());
            mPQ = tmp;
        }
    }
    @Override
    public int compareTo(MusicList o) {
        return (this.total > o.total) ? -1 : 1;
    }
}
class Music implements Comparable<Music>{
    int id;
    int plays;
    public Music(int id, int plays) {
        this.id = id;
        this.plays = plays;
    }
    @Override
    public int compareTo(Music o) {
        if(this.plays > o.plays || this.plays == o.plays && this.id < o.id)
            return -1;
        return this.plays > o.plays ? -1 : 1;
    }
}
```

