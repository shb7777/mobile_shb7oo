# mobile_shb7oo
def lfsr_simple():
    L = int(input("L: "))
    S = list(map(int, input("taps: ").split()))
    R = list(map(int, input("init: ")))
    m = int(input("m: "))

    ks = []
    for _ in range(m):
        ks.append(R[0])
        fb = R[S[0]] ^ R[S[1]]
        R = R[1:] + [fb]

    print("Keystream:", ks)

lfsr_simple()

###
def lfsr(r, t):
    fb = 0
    for i in t: fb ^= r[i]
    out = r[-1]
    return [fb] + r[:-1], out

def combine(a, b, c): return (a & b) ^ c

text = input("text: ")
bits = [int(b) for c in text for b in bin(ord(c))[2:].zfill(8)]

r1 = list(map(int, input("r1: ")))
r2 = list(map(int, input("r2: ")))
r3 = list(map(int, input("r3: ")))

t1 = list(map(int, input("t1: ").split()))
t2 = list(map(int, input("t2: ").split()))
t3 = list(map(int, input("t3: ").split()))

ks = []
for _ in bits:
    r1, x1 = lfsr(r1, t1)
    r2, x2 = lfsr(r2, t2)
    r3, x3 = lfsr(r3, t3)
    ks.append(combine(x1, x2, x3))

cipher = [p ^ k for p, k in zip(bits, ks)]

print("Keystream:", ks)
print("Cipher:", cipher)

###
def rc4(text, key, n):
    N = 2**n
    S = list(range(N))
    T = [key[i % len(key)] for i in range(N)]

    j = 0
    for i in range(N):
        j = (j + S[i] + T[i]) % N
        S[i], S[j] = S[j], S[i]

    i = j = 0
    ks = []
    for _ in text:
        i = (i + 1) % N
        j = (j + S[i]) % N
        S[i], S[j] = S[j], S[i]
        ks.append(S[(S[i] + S[j]) % N])

    return [t ^ k for t, k in zip(text, ks)], ks


text = [5,12,8,6,7,10,3]
key = [11,4,9,2,13]

c, ks = rc4(text, key, 4)

print("K:", ks)
print("C:", c)

###
