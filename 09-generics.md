# 9. Generics
Generics allow us to create classes, to design them, in a general way, without worrying about the elements they might contain. 

Java's ArrayLists are an example of a generic class, because many of the methods on that class can be applied to any type. 

Java supports generic types, such as classes/records/interfaces. It also supports generic methods. 

In a generic class definition, T is the placeholder for a type (type identifier) that will be specified later. In the declaration of a reference type that uses generics, the type parameter is declared in angel brackets, such as `ArrayList<String> listOfString`. Where the reference type is ArrayList, the type parameter is String, in the angle brackets. 

"Main.java":
```java
package dev.lpa;

interface Player {
    String name();
}
record BaseballPlayer(String name, String position) implements Player {}
record FootballPlayer(String name, String position) implements Player {}
record VolleyballPlayer(String name, String position) implements Player {}

public class Main {
    public static void main(String[] args) {
        var philly = new Affiliation("city", "Philadelphia, PA",
            "US");

        BaseballTeam phillies1 = new BaseballTeam("Philadelphia Phillies");
        BaseballTeam astros1 = new BaseballTeam("Houston Astros");
        scoreResult(phillies1, 3, astros1, 5);

        SportsTeam phillies2 = new SportsTeam("Philadelphia Phillies");
        SportsTeam astros2 = new SportsTeam("Houston Astros");
        scoreResult(phillies2, 3, astros2, 5);

        Team<BaseballPlayer, Affiliation> phillies =
                new Team<>("Philadelphia Phillies", philly);
        Team<BaseballPlayer, Affiliation> astros = new Team<>("Houston Astros");
        scoreResult(phillies, 3, astros, 5);

        var harper = new BaseballPlayer("B Harper", "Right Fielder");
        var marsh = new BaseballPlayer("B Marsh", "Right Fielder");
        phillies.addTeamMember(harper);
        phillies.addTeamMember(marsh);
        var guthrie = new BaseballPlayer("D Guthrie", "Center Fielder");
        phillies.addTeamMember(guthrie);
        phillies.listTeamMembers();

        SportsTeam afc1 = new SportsTeam ("Adelaide Crows");
        Team<FootballPlayer, String> afc = new Team<>("Adelaide Crows",
                "City of Adelaide, South Australia, in AU");
        var tex = new FootballPlayer("Tex Walker" , "Centre half forward");
        afc.addTeamMember(tex);
        var rory = new FootballPlayer("Rory Laird", "Midfield");
        afc.addTeamMember(rory);
        afc.listTeamMembers();

        Team<VolleyballPlayer, Affiliation> adelaide = new Team<>("Adelaide Storm");
        adelaide.addTeamMember(new VolleyballPlayer("N Roberts", "Setter"));
        adelaide.listTeamMembers();

        var canberra = new Team<VolleyballPlayer, Affiliation>("Canberra Heat");
        canberra.addTeamMember(new VolleyballPlayer("B Black", "Opposite"));
        canberra.listTeamMembers();
        scoreResult(canberra, 0, adelaide, 1);

//        Team<Integer> melbourneVB = new Team<>("Melbourne Vipers");
    }
    
    public static void scoreResult(BaseballTeam team1, int t1_score,
                                   BaseballTeam team2, int t2_score) {
        String message = team1.setScore(t1_score, t2_score);
        team2.setScore(t2_score, t1_score);
        System.out.printf("%s %s %s %n", team1, message, team2);
    }

    public static void scoreResult(SportsTeam team1, int t1_score,
                                   SportsTeam team2, int t2_score) {
        String message = team1.setScore(t1_score, t2_score);
        team2.setScore(t2_score, t1_score);
        System.out.printf("%s %s %s %n", team1, message, team2);
    }

    public static void scoreResult(Team team1, int t1_score,
                                   Team team2, int t2_score) {
        String message = team1.setScore(t1_score, t2_score);
        team2.setScore(t2_score, t1_score);
        System.out.printf("%s %s %s %n", team1, message, team2);
    }
}
```

"Team.java":
```java
package dev.lpa;

import java.util.ArrayList;
import java.util.List;

record Affiliation(String name, String type, String countryCode) {
    @Override
    public String toString() {
        return name + " (" + type + " in " + countryCode + ")";
    }
}

public class Team<T extends Player, S> {
    private String teamName;
    private List<T> teamMembers = new ArrayList<>();
    private int totalWins = 0;
    private int totalLosses = 0;
    private int totalTies = 0;
    private S affiliation;

    public Team(String teamName) {
        this.teamName = teamName;
    }

    public Team(String teamName, S affiliation) {
        this.teamName = teamName;
        this.affiliation = affiliation;
    }

    public void addTeamMember(T t) {
        if (!teamMembers.contains(t)) {
            teamMembers.add(t);
        }
    }

    public void listTeamMembers() {
        System.out.print(teamName + " Roster:");
        System.out.println((affiliation == null ? "" : " AFFILIATION: "+ affiliation));
        for (T t : teamMembers) {
            System.out.println(t.name());
        }
    }

    public int ranking() {
        return (totalLosses * 2) + totalTies + 1;
    }

    public String setScore(int ourScore, int theirScore) {
        String message = "lost to";
        if (ourScore > theirScore) {
            totalWins++;
            message = "beat";
        } else if (ourScore == theirScore) {
            totalTies++;
            message = "tied";
        } else {
            totalLosses++;
        }

        return message;

    }

    @Override
    public String toString() {
        return teamName + " (Ranked "  + ranking() + ")";
    }
}
```

"BaseballTeam.java":
```java
package dev.lpa;

import java.util.ArrayList;
import java.util.List;

public class BaseballTeam {
    private String teamName;
    private List<BaseballPlayer> teamMembers = new ArrayList<>();
    private int totalWins = 0;
    private int totalLosses = 0;
    private int totalTies = 0;

    public BaseballTeam(String teamName) {
        this.teamName = teamName;
    }

    public void addTeamMember(BaseballPlayer player) {
        if (!teamMembers.contains(player)) {
            teamMembers.add(player);
        }
    }

    public void listTeamMembers() {
        System.out.println(teamName + " Roster:");
        System.out.println(teamMembers);
    }

    public int ranking() {
        return (totalLosses * 2) + totalTies + 1;
    }

    public String setScore(int ourScore, int theirScore) {
        String message = "lost to";
        if (ourScore > theirScore) {
            totalWins++;
            message = "beat";
        } else if (ourScore == theirScore) {
            totalTies++;
            message = "tied";
        } else {
            totalLosses++;
        }

        return message;
    }

    @Override
    public String toString() {
        return teamName + " (Ranked "  + ranking() + ")";
    }
}
```

"SportsTeam.java":
```java
package dev.lpa;

import java.util.ArrayList;
import java.util.List;

public class SportsTeam {
    private String teamName;
    private List<Player> teamMembers = new ArrayList<>();
    private int totalWins = 0;
    private int totalLosses = 0;
    private int totalTies = 0;

    public SportsTeam(String teamName) {
        this.teamName = teamName;
    }

    public void addTeamMember(Player player) {
        if (!teamMembers.contains(player)) {
            teamMembers.add(player);
        }
    }

    public void listTeamMembers() {
        System.out.println(teamName + " Roster:");
        System.out.println(teamMembers);
    }

    public int ranking() {
        return (totalLosses * 2) + totalTies + 1;
    }

    public String setScore(int ourScore, int theirScore) {
        String message = "lost to";
        if (ourScore > theirScore) {
            totalWins++;
            message = "beat";
        } else if (ourScore == theirScore) {
            totalTies++;
            message = "tied";
        } else {
            totalLosses++;
        }

        return message;
    }

    @Override
    public String toString() {
        return teamName + " (Ranked "  + ranking() + ")";
    }
}

```
























