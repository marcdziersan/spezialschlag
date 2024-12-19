```
// Neue Fähigkeiten und Statuseffekte im Kampfsystem
function performSpecialMove(player, enemy, moveType) {
    // Statuseffekt-Handler
    const applyStatusEffect = (target, effect, duration) => {
        target.statusEffects.push({ effect, duration });
        console.log(`${target.name} ist von ${effect} betroffen für ${duration} Runden.`);
    };

    // Effekt auflösen
    const resolveStatusEffects = (character) => {
        character.statusEffects = character.statusEffects.filter(effect => {
            console.log(`${character.name} ist immer noch von ${effect.effect} betroffen.`);
            effect.duration--;
            if (effect.effect === "Gift") {
                character.health -= 5; // Gift verursacht Schaden
                console.log(`${character.name} erleidet 5 Giftschaden.`);
            }
            if (effect.effect === "Blutung") {
                character.health -= 8; // Blutung verursacht höheren Schaden
                console.log(`${character.name} erleidet 8 Blutungsschaden.`);
            }
            return effect.duration > 0;
        });
    };

    // Verfügbare Spezialangriffe
    const specialMoves = {
        "Kraftschlag": () => {
            const damage = player.strength * 2 - enemy.defense;
            console.log(`${player.name} führt einen Kraftschlag aus und verursacht ${damage} Schaden.`);
            enemy.health -= Math.max(damage, 0);
        },
        "Betäuben": () => {
            console.log(`${player.name} versucht ${enemy.name} zu betäuben.`);
            if (Math.random() < 0.5) { // 50% Chance auf Erfolg
                applyStatusEffect(enemy, "Betäubt", 1);
            } else {
                console.log(`${player.name} hat den Betäuben-Angriff verfehlt.`);
            }
        },
        "Vergiften": () => {
            console.log(`${player.name} vergiftet ${enemy.name}.`);
            applyStatusEffect(enemy, "Gift", 3);
        },
        "Blutungsschlag": () => {
            const damage = player.strength * 1.5 - enemy.defense;
            console.log(`${player.name} schlägt und verursacht ${damage} Schaden mit Blutung.`);
            enemy.health -= Math.max(damage, 0);
            applyStatusEffect(enemy, "Blutung", 2);
        }
    };

    // Spezialangriff ausführen
    if (specialMoves[moveType]) {
        specialMoves[moveType]();
    } else {
        console.log("Unbekannte Spezialfähigkeit!");
    }

    // Statusauflösungen am Ende der Runde
    resolveStatusEffects(player);
    resolveStatusEffects(enemy);
}

// Beispielaufruf im Kampf
const player = {
    name: "Held",
    health: 100,
    strength: 20,
    defense: 10,
    statusEffects: [],
    level: 5
};

const enemy = {
    name: "Goblin",
    health: 60,
    strength: 15,
    defense: 8,
    statusEffects: []
};

// Spieler kann Fähigkeiten basierend auf Level nutzen
if (player.level >= 3) performSpecialMove(player, enemy, "Kraftschlag");
if (player.level >= 5) performSpecialMove(player, enemy, "Betäuben");
if (player.level >= 7) performSpecialMove(player, enemy, "Vergiften");
if (player.level >= 10) performSpecialMove(player, enemy, "Blutungsschlag");

console.log("Kampfrunde abgeschlossen.");
```
