# project_java
package com.company;

import java.util.*;

public class LadderGame {

    public int playerMaxNum = 0; //플레이어 최대수
    public int level;
    public int locationOfPlayer = 0;

    public final String DOWN = "D";
    public final String TURN_LEFT = "L";
    public final String TURN_RIGHT = "R";

    ArrayList playerList = new ArrayList<String>();
    ArrayList rewardList = new ArrayList<String>();

    String[][] ladderArray; //사다리배열

    LadderGame(int level){
        this.level = level;
    }

    // 플레이어의 수를 입력받기
    public void inputPlayerNum() {

        // 플레이어수를 사용자에게 입력받음
        Scanner scan = new Scanner(System.in);

        while (playerMaxNum < 2 || playerMaxNum > 10) {
            System.out.println("최소2명, 최대 10명까지 플레이할 수 있습니다. " +
                    "플레이어 수를 입력하세요");

            // 입력받은 플레이어 수가 숫자가 아니면 다시받도록 반복
            while (!scan.hasNextInt()) {
                scan.next();
                System.out.println("숫자가 아닙니다. 다시 입력하세요: ");
            }
            playerMaxNum = scan.nextInt();
        }

        System.out.println("The Number of Players: " + playerMaxNum);
        ladderArray = new String[level][playerMaxNum];

    }

    // 플레이어의 이름과 당첨항목의 이름을 입력받기
    public void inputNames() {

        Scanner scan = new Scanner(System.in);

        for (int i = 0; i < playerMaxNum; i++) {
            System.out.println("플레이어의 이름을 입력하세요.");

            playerList.add(scan.nextLine());
        }

        for (int i = 0; i < playerMaxNum; i++) {
            System.out.println("PLAYER" + (i + 1) + " : " + playerList.get(i));
        }

        for (int i = 0; i < playerMaxNum; i++) {
            System.out.println("당첨항목을 입력하세요.");

            rewardList.add(scan.nextLine());
        }

        for (int i = 0; i < playerMaxNum; i++) {
            System.out.println("REWARD" + (i + 1) + " : " + rewardList.get(i));
        }
    }

    //사다리를 방향으로 정의.지정
    public void ladderMaking() {

        //사다리의 branch를 랜덤으로 발생
        Random ladderRandom = new Random(); //아래,양 옆의 방향은 랜덤

        for (int i = 0; i < level; i++) {
            int ladderDirection = ladderRandom.nextInt(playerMaxNum - 1);

            for (int j = 0; j < playerMaxNum; j++) {

                if (j == ladderDirection) {
                    ladderArray[i][j] = TURN_RIGHT;
                } else if (j == ladderDirection + 1) {
                    ladderArray[i][j] = TURN_LEFT;
                } else {
                    ladderArray[i][j] = DOWN;
                }
                System.out.print(ladderArray[i][j] + " ");

            } System.out.println();
        }
    }

    //플레이어가 당첨항목으로 이동하는 과정
    public void matchingPlayerWithReward() {

        HashMap playerAndRewardMap = new HashMap();

        for (int i = 0; i < playerMaxNum; i++) {
            locationOfPlayer = i;
            for (int j = 0; j < level; j++) {
            if (ladderArray[j][locationOfPlayer].equals(TURN_RIGHT) ) {
                    locationOfPlayer++;

                } else if (ladderArray[j][locationOfPlayer].equals(TURN_LEFT)) {
                    locationOfPlayer--;
                }
            }

            playerAndRewardMap.put(playerList.get(i), rewardList.get(locationOfPlayer));
        }
        System.out.println(playerAndRewardMap);
    }

    // 게임을 실행
    public void run() {
        inputPlayerNum();
        inputNames();
        ladderMaking();
        matchingPlayerWithReward();
    }
}

// in other javaClass named Main
package com.company;

public class Main{


    public static void main(String[] args) {

        // LadderGame의 생성자와 level에 값 부여
        LadderGame ladderGame = new LadderGame(5);

        ladderGame.run();

        System.out.println();
    }
}
