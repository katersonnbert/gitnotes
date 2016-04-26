OAuth 2
=======

# OAuth key concepts

## Primary concepts

- TOFU ... trust on first use ... user has to make the decision once to allow a client to act on his behalf for a certain action. after this first grant, the client can act w/o asking the client again. 

## Primary actors

- Resource owner (RO)
- Client (C)
- OAuth server (AS)
- Protected resource / Resource server (RS)

## Primary workflow

- C wants to gain access to a resource using a C
- C requests authorization from RO
- RO grants authorization to C
- C sends authorization grant to AS
- AS sends access token to C
- C sends access token to RS
- RS sends resource to C

## Detailed workflow

- C redirects RO to authorization endpoint (AS)
- RO authenticates to AS (TOFU)
- RO grants authorization to C
- AS redirects RO to C with authorization code
- C sends authorization code to AS token endpoint
- AS sends token to C
- C sends token to RS
- RS sends resource to C

