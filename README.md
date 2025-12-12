# Count-Mentions-Per-User

You are given an integer numberOfUsers representing the total number of users and an array events of size n x 3.

Each events[i] can be either of the following two types:

Message Event: ["MESSAGE", "timestampi", "mentions_stringi"]
This event indicates that a set of users was mentioned in a message at timestampi.
The mentions_stringi string can contain one of the following tokens:
id<number>: where <number> is an integer in range [0,numberOfUsers - 1]. There can be multiple ids separated by a single whitespace and may contain duplicates. This can mention even the offline users.
ALL: mentions all users.
HERE: mentions all online users.
Offline Event: ["OFFLINE", "timestampi", "idi"]
This event indicates that the user idi had become offline at timestampi for 60 time units. The user will automatically be online again at time timestampi + 60.
Return an array mentions where mentions[i] represents the number of mentions the user with id i has across all MESSAGE events.

All users are initially online, and if a user goes offline or comes back online, their status change is processed before handling any message event that occurs at the same timestamp.

Note that a user can be mentioned multiple times in a single message event, and each mention should be counted separately.

class Solution:
    def countMentions(self, n: int, events: List[List[str]]) -> List[int]:
        mentions = [0]*n
        back = [0]*n
        events.sort(key=lambda e: (int(e[1]), e[0]=="MESSAGE"))

        for typ, t, data in events:
            t = int(t)
            if typ == "OFFLINE":
                back[int(data)] = t + 60
                continue

            for tok in data.split():
                if tok == "ALL":
                    for u in range(n):
                        mentions[u] += 1
                elif tok == "HERE":
                    for u in range(n):
                        if t >= back[u]:
                            mentions[u] += 1
                else:  
                    mentions[int(tok[2:])] += 1

        return mentions
