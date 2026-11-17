# Company Brain: the prompts

These are the prompts I used, in order, to build a "company brain" with Claude Code. It's a shared knowledge base that coding agents (Claude Code, Codex, ChatGPT) and other agents can read and write through MCP, with a web app for browsing it. It's hosted on Cloudflare (Workers, D1, Vectorize, Access) and fits the free / $5 plan.

I'm sharing them so you can see how a non-expert can steer an agent through a real build: start broad, let it research, answer its questions, then approve each step.

Personal and company details are swapped for placeholders (`yourcompany.com`, `brain.yourcompany.com`). Otherwise the prompts are word for word, typos included.

---

## 1. The opening brief

> I'd like to build a company brain solution. I know I want it to be able to interface with my different coding agents. I use Claude Code, Codex, and ChatGPT. I also want to be able to talk to my second brain or company brain through non-coding agents. It needs to be able to talk to a muse, a Grok bot, and those types of things.
>
> I know I want to deploy it. I want a web frontend where I can view and browse the brain, and I want that to be on Vercel or Cloudflare. I'm not sure yet which one. I also want my teammates to be able to access it, so I want other users to be able to access and log in to their company brain through their own agents of choice, all the ones I listed as well.
>
> I'm not really sure if there's anything else that I want. Maybe help me do some web research on other company brain and second brain solutions, especially ones that are self-built. Help me figure out what other features or things I might want in this application. I'm really not sure about what I want, so I really need some help in terms of grounding research and data on features that can make this robust, follow best practices, and be secure (all of those fun things). I'm not really a developer with a ton of experience, so I need to understand what's important when I build this thing.

**What happened:** Claude researched existing company-brain and second-brain tools, explained the options, and asked whether to build new or adapt something that already exists.

## 2. Build or adopt

> Build new

## 3. Answering Claude's setup questions

Claude asked three multiple-choice questions. My answers:

- **Which stack?** "I'd like to research what's available on the free and paid plans. If the Cloudflare all-in is possible in the $5 plan or the free plan, we should do that, but if Vercel and Supabase is the only free option, then we should do that."
- **First milestone?** "Core loop (Recommended)"
- **Project name?** "company-brain"

**What happened:** Claude confirmed that Cloudflare's free and $5 plans cover everything, then wrote the first plan.

## 4. Writing the plan down

> put all of our plan details into PLAN.md, including what is remaining, what features are in different phases and status of implementation

> what folder is the plan file in?

## 5. Deciding who can do what

Claude asked how permissions should work before writing that code. My reply:

> All right, I'm going to answer your part, or the "can" for the permissions file.
>
> * Can admins read everyone's private spaces? I will say yes. Admins should be able to see everything by default.
> * In a company space, can every member write, or only read unless made an editor? I would say every member can write for now. We can always restrict permissions later.
> * Who can review agent proposals? Let's just say everyone can write. Everyone who can write can review, and then default anything that's not explicit to return false, meaning people who are not granted this permission cannot see everything by default. I think that's a good plan, so yes, let's go with that as well.
> * For the Google OAuth sign-in, I don't really want to set that up right this second, so let's just use Cloudflare access so people can sign in with their OTP codes or magic links or whatever Cloudflare uses, just for the first version of this.
>
> Ask me any other questions you have before building.

That last line matters. It asks the agent to raise its open questions *before* it writes code, not halfway through.

## 6. Claude's follow-up questions

- **Which address should the brain use?** "brain.yourcompany.com (Recommended)"
- **Who should Cloudflare Access let in?** "@yourcompany.com + listed emails (Recommended)"
- **Do you already have a Cloudflare Zero Trust team?** "I'm not sure if I have one set up. Why don't you check for me and let me know if I need to do anything?"
- **Who should be an admin?** "Just me"

## 7. Creating the cloud resources

> Yes, create them

> All right, I think I already have Cloudflare Zero Trust set up. I see the Cloudflare one with the users list here, so let me know if I need to do anything else specifically.

> Let's use the in-app browser for this instead of me doing things in the browser. Why don't you open up in the in-app browser and drive?

**What happened:** Claude opened the Cloudflare dashboard in its built-in browser. I signed in myself (the agent never types passwords), then Claude set up the Access apps.

> done

## 8. Ship it

> Yes, test and deploy

> what is the link to our github repo here?

> Yes, create private repo

**What happened:** automated security reviews flagged two ways an approved agent proposal could overwrite an existing page. Claude fixed both on a branch with tests, then asked before shipping.

> and yes after that, push the fix, open PR, then migrate and deploy

---

## Takeaways

1. **Say what you don't know.** "I'm not really a developer" and "help me do some web research" got me a research pass and explanations instead of code I couldn't judge.
2. **Make the decisions that are yours.** Who sees what, who is admin, and which sign-in method are business calls. The agent asked; I answered in plain English.
3. **Ask for questions up front.** "Ask me any other questions you have before building" saves rework.
4. **Keep a living plan.** `PLAN.md` holds phases, status and what's left, so any session (or person) can pick it up.
5. **Approve the risky steps one at a time.** Creating cloud resources, deploying, making repos and pushing each got an explicit "yes".
6. **Keep secrets out of the agent's hands.** I signed in to Cloudflare myself in the shared browser.
7. **Let security review run, and act on it.** The automated reviews caught real bugs before anyone used the app.
