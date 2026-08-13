# DATA_MODELS.md — Esquemas base

```ts
type Character = {
  id: string;
  userId: string;
  name: string;
  avatarUrl: string;
  faction: string;
  class: "Guerrero" | "Pícaro" | "Mago" | "Paladín" | "Comodín";
  attributes: {
    fuerza: number;
    destreza: number;
    inteligencia: number;
    vitalidad: number;
    suerte: number;
  };
  level: number;
  xp: number;
  hp: number;
  maxHp: number;
  mana: number;
  maxMana: number;
  position: { map: string; x: number; y: number };
  createdAt: string;
};

type Faction = {
  id: string;
  name: string;
  color: string;
  lore: string;
  territoryControlled: number; // 0-100
};

type Quest = {
  id: string;
  title: string;
  description: string;
  factionOrigin: string;
  objectiveType: "kill_enemy" | "explore_zone" | "defeat_player" | "escort";
  objectiveTarget: string;
  reward: { xp: number; gold: number; itemId: string | null };
  expiresAt: string;
  status: "available" | "in_progress" | "completed" | "expired";
};

type BattleState = {
  id: string;
  participantsA: string[]; // character ids
  participantsB: string[];
  turnOrder: string[];
  log: BattleEvent[];
  status: "ongoing" | "won_a" | "won_b" | "fled";
};

type Party = {
  id: string;
  leaderId: string;
  memberIds: string[];
  maxSize: number;
};
```

Guarda esto en `shared/types/` y úsalo tanto en server como en cliente
(si usas TypeScript) — evita el bug clásico de que el shape del JSON que
manda el server no coincide con lo que espera el cliente.
