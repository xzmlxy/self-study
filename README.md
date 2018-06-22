# something_for_leetcode 813
def get_max_sum(A, K):
    length = len(A)
    all = [[None for i in range(length)] for j in range(K)]
    temp = 0
    for i in range(0, length - K + 1):
        temp += A[i]
        all[0][i] = temp / (i + 1)
    i = 1
    while i + 1 < K:
        for m in range(i, length - K + i + 1):
            tem_max = all[i - 1][m - 1] + A[m]
            temp = A[m]
            for n in range(m - 2, i - 2, -1):
                temp += A[n + 1]
                tem_max = max(tem_max, all[i - 1][n] + temp / (m - n))
            all[i][m] = tem_max
        i += 1
    print(all)
    result = all[i - 1][length - 2] + A[-1]
    temp = A[-1]
    for n in range(length - 3, K - 3, -1):
        temp += A[n + 1]
        result = max(result, all[i - 1][n] + temp / (length - n - 1))
    return result
