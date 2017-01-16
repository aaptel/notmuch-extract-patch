# Usage

Let's say you use notmuch as your email client and you want to try out
one of those git patchset sent on a mailing list with all diffs
grouped in a thread, one patch per email. ("[PATCH 1/15] do blah...").

Simply exporting the thread is not enough, you need to skip any
feedbacks or cover letters that have been posted on the thread.

This is what this script does.

Example usage:

    $ notmuch-extract-patchset thread:000000000000265f > feature.patchset
    $ git checkout -b test-feature
    $ git am feature.patchet


You can use the following to use it directly from emacs:

    (defun apply-thread-patchset (repo branch)
      (interactive "Dgit repo: \nsnew branch name: ")
      (let ((tid notmuch-show-thread-id)
        (tmp "/tmp/notmuch-patchset"))
        (shell-command (format "notmuch-extract-patch %s > %s && ( cd %s && git checkout -b %s && git am %s )"
                               (shell-quote-argument tid)
                               (shell-quote-argument tmp)
                               (shell-quote-argument (expand-file-name repo))
                               (shell-quote-argument branch)
                               (shell-quote-argument tmp)))))
