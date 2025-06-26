-  Access control vulnerability where you can access resources you wouldn't ordinarily be able to see. This occurs when the programmer exposes a Direct Object Reference, which is just an identifier that refers to specific objects within the server. By object, we could mean a file, a user, a bank account in a banking application, or anything really.

![[IDOR_1.png]]

By changing the id, bank information related to that account id will be displayed.

![[IDOR_2.png]]