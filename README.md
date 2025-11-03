import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = Integer.parseInt(sc.nextLine().trim());
        for (int tc = 0; tc < T; tc++) {
            // read number of passwords
            int n = Integer.parseInt(sc.nextLine().trim());
            // read n passwords (they may be on one line or split across lines)
            List<String> dict = new ArrayList<String>(n);
            while (dict.size() < n) {
                String[] parts = sc.nextLine().trim().split("\\s+");
                for (String p : parts) {
                    if (p.length() > 0) dict.add(p);
                    if (dict.size() == n) break;
                }
            }
            // read the login attempt
            String login = sc.nextLine().trim();

            // solve
            List<String> sequence = new ArrayList<String>();
            Boolean[] memo = new Boolean[login.length() + 1]; // memo[i] == false means starting at i is impossible
            boolean ok = crack(login, dict, 0, memo, sequence);

            if (!ok) {
                System.out.println("WRONG PASSWORD");
            } else {
                // join sequence with spaces
                StringBuilder out = new StringBuilder();
                for (int i = 0; i < sequence.size(); i++) {
                    if (i > 0) out.append(" ");
                    out.append(sequence.get(i));
                }
                System.out.println(out.toString());
            }
        }
        sc.close();
    }

    // return true if s from index idx can be cracked; sequence collects the words
    private static boolean crack(String s, List<String> dict, int idx, Boolean[] memo, List<String> sequence) {
        if (idx == s.length()) return true;
        if (memo[idx] != null && memo[idx] == false) return false;

        for (String w : dict) {
            int len = w.length();
            if (idx + len <= s.length() && s.startsWith(w, idx)) {
                sequence.add(w);
                if (crack(s, dict, idx + len, memo, sequence)) {
                    return true;
                }
                // backtrack
                sequence.remove(sequence.size() - 1);
            }
        }

        memo[idx] = false; // no word leads to a solution from idx
        return false;
    }
}
