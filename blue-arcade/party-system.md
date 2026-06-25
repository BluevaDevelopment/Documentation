# Party System

The party system allows players to group together and play games as a team. When a party leader joins an arena, all party members join together.

## Overview

- **Party Leader**: The player who created or currently leads the party.
- **Party Members**: Players who have joined the party.
- **Party Types**: Public (anyone can join) or Private (invite only).

**Permission Required:** `bluearcade.party`

---

## Commands

All party commands use `/ba party` or just `/ba party` to open the menu.

### Creating a Party

```
/ba party create
```

Creates a new party with you as the leader. The party starts as **private** by default.

### Inviting Players

```
/ba party invite [player]
```

Sends an invitation to the specified player. They must accept to join.

**Or use the menu:**
```
/ba party invite
```

Opens a menu showing online players you can invite.

### Accepting/Rejecting Invitations

```
/ba party accept [leader_name]
/ba party reject [leader_name]
```

When you receive an invitation, use the leader's name to accept or reject it.

### Party Management (Leader Only)

**Kick a member:**
```
/ba party kick [player]
```

**Transfer leadership:**
```
/ba party leader [player]
```

**Set party type:**
```
/ba party type <public|private>
```

### Leaving a Party

```
/ba party leave
```

If the leader leaves, leadership transfers to another member. If the leader disconnects, a reconnect timeout applies (default 60 seconds) before the party is disbanded. If you are the last member, the party is disbanded.

### Viewing Party Info

```
/ba party list
```

Shows all party members in chat.

```
/ba party members (page)
```

Opens a menu showing all members.

### Public Parties

**Search for public parties:**
```
/ba party search (page)
```

**Join a public party:**
```
/ba party join [leader_name]
```

---

## Menu Navigation

### Main Party Menu
```
/ba party
/ba party menu
```

Opens the main party interface with options to:
- Create a party
- View/manage members
- Invite players
- Search public parties
- Leave party

### Members Menu
```
/ba party members
/ba party members page [number]
```

View all party members. Leaders can kick members or transfer leadership from this menu.

### Invite Menu
```
/ba party invite
/ba party invite page [number]
```

Browse online players and send invitations.

### Public Search
```
/ba party search
/ba party search page [number]
```

Find and join public parties.

---

## How Parties Work in Games

### Joining Arenas

When the party leader joins an arena:
1. All online party members are brought along.
2. The arena must have enough space for the entire party.
3. If the arena is full, the join fails.

### During Games

- Party members play together in the same arena.
- In team-based games, party members may be placed on the same team if the module supports it.
- Individual scores are still tracked.

### Leaving Arenas

- If the leader leaves, all party members leave.
- Individual members can leave without affecting others.
- The party stays intact after games end.

---

## Party Types

### Private Party (Default)

- Only invited players can join.
- The leader must send invitations.
- More control over who plays with you.

### Public Party

- Anyone can join without an invitation.
- Visible in public party search.
- Good for meeting new players.

**Change type:**
```
/ba party type public
/ba party type private
```

---

## Tips

1. **Create a party before joining** — Form your group in the lobby.
2. **Check party size** — Make sure the arena can fit your entire party.
3. **Use public parties** — Great way to find players when playing solo.
4. **Leadership transfer** — Transfer leadership before leaving if you want the party to continue.

---

## Permissions

| Permission | Description |
|------------|-------------|
| `bluearcade.party` | Access to all party features |

---

## Troubleshooting

**"Party is full"**
- Parties are capped by the largest configured arena capacity.
- The leader should join a larger arena.

**"Player is already in a party"**
- The invited player must leave their current party first.
- Or they need to reject their pending invitations.

**"Arena doesn't have enough space"**
- The arena cannot fit all party members.
- Try a different arena with a higher max players setting.

**"You are not the party leader"**
- Only the leader can kick, invite, or transfer leadership.
- Ask the leader to transfer leadership to you.
