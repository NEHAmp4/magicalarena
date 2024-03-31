import java.util.Random;

class Player {
    private int health;
    private int strength;
    private int attack;
    private Random random;

    public Player(int health, int strength, int attack) {
        this.health = health;
        this.strength = strength;
        this.attack = attack;
        this.random = new Random();
    }

    public int getHealth() {
        return health;
    }

    public void setHealth(int health) {
        this.health = health;
    }

    public int getStrength() {
        return strength;
    }

    public int getAttack() {
        return attack;
    }

    public int rollDice() {
        return random.nextInt(6) + 1;
    }
}

public class MagicalArena {
    public static void fight(Player playerA, Player playerB) {
        while (playerA.getHealth() > 0 && playerB.getHealth() > 0) {
            Player attacker = playerA.getHealth() < playerB.getHealth() ? playerA : playerB;
            Player defender = attacker == playerA ? playerB : playerA;

            int attackRoll = attacker.rollDice();
            int defenseRoll = defender.rollDice();

            int attackDamage = attackRoll * attacker.getAttack();
            int defenseStrength = defenseRoll * defender.getStrength();

            int damageDealt = Math.max(attackDamage - defenseStrength, 0);
            defender.setHealth(defender.getHealth() - damageDealt);

            System.out.println(attacker == playerA ? "Player A" : "Player B" + " attacks with roll " + attackRoll + " and deals " + damageDealt + " damage to " + (defender == playerA ? "Player A" : "Player B") + ". " + (defender == playerA ? "Player A" : "Player B") + "'s health: " + defender.getHealth());
        }

        System.out.println(playerA.getHealth() > 0 ? "Player A wins!" : "Player B wins!");
    }
}





Unit Tests:
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MagicalArenaTest {
    @Test
    public void testFight() {
        Player playerA = new Player(50, 5, 10);
        Player playerB = new Player(100, 10, 5);

        MagicalArena.fight(playerA, playerB);

        assertTrue(playerA.getHealth() <= 0 || playerB.getHealth() <= 0);
    }

    @Test
    public void testRollDice() {
        Player player = new Player(100, 10, 5);

        int roll = player.rollDice();

        assertTrue(roll >= 1 && roll <= 6);
    }
}
