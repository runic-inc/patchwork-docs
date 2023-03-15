## Example json

Application fragments and rules can be expressed in json:

```[
    {
        "id": 1,
        "key": "atributedIDs",
        "type": "uint64",
        "arrayLength": 8,
        "description": "The attributes that comprise this NFT"
    },
    {
        "id": 2,
        "key": "xp",
        "type": "uint16",
        "description": "This NFT's XP"
    },
    {
        "id": 3,
        "key": "level",
        "type": "uint8",
        "description": "This NFT's level"
    },
    {
        "id": 4,
        "key": "xpLost",
        "type": "uint16",
        "description": "How much XP this NFT has lost"
    },
    {
        "id": 5,
        "key": "stakedMade",
        "type": "uint16",
        "description": "How many questions this NFT has staked"
    },
    {
        "id": 6,
        "key": "stakedCorrect",
        "type": "uint16",
        "description": "How many staked questions this NFT has made correctly"
    },
    {
        "id": 7,
        "key": "evolution",
        "type": "uint8",
        "description": "The evolution level of this NFT"
    },
    {
        "id": 8,
        "key": "nickname",
        "type": "char16",
        "description": "This NFT's nick name"
    }
]
```
