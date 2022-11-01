+++
author = "Alex Bilson"
date = "2022-10-13 10:31:26"
lastmod = "2022-10-13 10:39:09"
epistemic = "plant"
tags = ["python","cli"]
+++
Python is a great language. Sometimes, however, you need to interact directly with the operating system. Like in this case, when I needed to run a few Git commands, or if I'd written a Rustlang CLI. Turns out, a pretty simple problem to solve.

{{< highlight python >}}
def try_run_cmd(cmds: List[str], cwd: str) -> Tuple[str, str]:
    output = None
    try:
        output = subprocess.run(cmds, capture_output=True, check=True, cwd=cwd)
    except CalledProcessError as e:
        return None, f"{e} : {e.output}"

	  return output.stdout.decode(), output.stderr.decode()

# and usage (the -v flag sends output back for validation)
try_run_cmd(["git", "add", "-v", "."], "/app")
{{< /highlight >}}

{{< notice type=warning >}}
If you use this for Git, know that "git push" flips stdout and stderr. Oye ve.
{{< /notice >}}
