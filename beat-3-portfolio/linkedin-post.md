I opened my first fix in a real open-source project: quality-time, a tool teams use to monitor the quality of their software. My change makes one of its services report itself as unhealthy when its work piles up, instead of saying everything is fine.

Before writing any code, I reproduced the problem, then wrote a test that failed until my fix was in place. I also built small tools to help me work carefully: one that checks what I post before it goes out, and one that runs the project's own checks before I submit. That second one caught a mistake in my change before anyone else saw it.

The project's lead maintainer reviewed my pull request and asked for two improvements, including one that makes problems easier to debug. It's still open, and I'm making those changes now.

The biggest lesson: share a plan early. Feedback before I build is cheaper than feedback after.

https://github.com/ICTU/quality-time/pull/14917

#OpenSource #Python #SoftwareEngineering
