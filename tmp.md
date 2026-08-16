
__Prior to ClientHello__
Client generates his Key-Pair and Curve Parameters P
(z.B. Curve25519)
• $k_{secret\_client}$ = Random value (256 Bit)
• $k_{public\_client}$ = $P$ * $k_{secret\_client}$

__Prior to ServerHello__
Server generates his Key-Pair with P sent by Client
• $k_{secret\_server}$ = Random Value (256 Bit)
• $k_{public\_server}$ = $P$ * $k_{secret\_server}$

__->Key Generation__
$k_{shared\_secret}$ = $k_{public\_client}$ * $k_{private\_server}$
