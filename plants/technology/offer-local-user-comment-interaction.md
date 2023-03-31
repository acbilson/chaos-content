+++
author = "Alex Bilson"
date = "2023-03-31 15:30:18"
lastmod = "2023-03-31 15:33:08"
epistemic = "plant"
tags = ["snippet","javascript","html"]
+++
I'm wondering if it would be nice for users to take comments on my pages? Or for myself even? Here's a self-contained example of a simple page that lets the user highlight a bit of text, make a short comment, and append it to the page. I'm not interested in wiring this up to a service or anything, but it's pretty cool just to have this kind of local interaction with the content.

{{< highlight html >}}
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>Document</title>
		<style>
			:root {
				--spread-gap: 0.5rem;
				--flow-gap: 1em;
			}
			dialog {
				width: 80%;
				min-width: 1vw;
			}

			.spread {
				display: flex;
				flex-flow: row nowrap;
				column-gap: var(--spread-gap);
			}

			.spread-down {
				display: flex;
				flex-flow: column nowrap;
				row-gap: var(--spread-gap);
			}

			.flow > * + * {
				margin-top: var(--flow-gap);
			}

			footer p.commented {
				font-style: italic;
			}

			footer p.commenter {
			}
		</style>
	</head>
	<body>
		<main>
			<p>
				When a corporation starts, its members rush to find customers, to meet
				their needs, to iterate, to create, to re-invent. As the business grows,
				they search for more customers with the problem their product/service
				solves. As their customer base expands, they tailor their solution to
				their customers needs. If their customers' needs change, they modify
				their product to match. One might think this cycle would continue
				forever. The slow, business-killing changes happen, not among one’s own
				customer base, but with those who are not even customers - the
				‘noncustomer’ (Drucker, referenced by Krames, pg. 95).
			</p>

			<p>
				Drucker puts it, “The first signs of fundamental change rarely appear
				within one’s own organization or among one’s own customers.” An
				established business may become so focused on serving its customers that
				it becomes oblivious to new opportunities or threats in the marketplace.
				Like the many companies whom new technology sunk (Kodak, Nokia,
				Blackberry), there is a danger for a company to grow insular to the
				wider marketplace. It can forget that noncustomers' decisions does
				affect their business, even if it’s not listed on in their bottom line.
			</p>

			<p>
				When a new way of doing things arises, it’s not one’s loyal customers
				who are likely to make a change. People in general don’t like to change,
				and if you only measure your business' success by the dedication of your
				customers, without taking into account whether new or different
				customers are using your product/service, you may find that you’ve been
				passed by the next entrepreneur.
			</p>
		</main>

		<h2>References</h2>
		<ul>
			<li>
				Krames, Jeffrey A. (2014) Lead with Humility: 12 Leadership Lessons from
				Pope Francis. AMACOM. Chapter 12: Pay Attention to Noncustomers.
			</li>
		</ul>
		<dialog>
			<p>You selected: <span class="content"></span></p>
			<form class="flow" method="dialog">
				<textarea
					class="spread-down"
					rows="5"
					cols="30"
					name="content"
					placeholder="Leave a comment here..."
					maxlength="50"
					required
				/></textarea>
				<label for="content" class="characters">Characters: 0/50</label>
				<div class="spread">
					<input type="submit" value="Make Comment" />
					<i>ESC to exit</i>
				</div>
			</form>
		</dialog>
		<footer>
			<ol id="comments">
			</ol>
		</footer>
		<script>
			const mainEl = document.querySelector("main");
			const dialogEl = document.querySelector("dialog");
			const dialogContentEl = document.querySelector("dialog .content");
			const dialogCommentEl = document.querySelector("dialog textarea");
			const characterEl = document.querySelector("dialog .characters");
			const commentsEl = document.querySelector("footer #comments");

			// captures highlighted text and shows dialog popup
			mainEl.addEventListener("mouseup", (e) => {
				e.preventDefault();
				let selectedText = window.getSelection().toString().trim();
				dialogContentEl.innerText = selectedText;
				if (selectedText !== "") dialogEl.showModal();
			});

			// submits comment from user
			dialogEl.addEventListener("submit", (e) => {
				e.preventDefault();
				let text = dialogContentEl.innerText;
				let comment = dialogCommentEl?.value;

				// resets comment text
				dialogCommentEl.value = "";
				characterEl.innerText = `Characters: 0/50`;

				//adds comment for visiblitity
				if (!commentsEl.children?.length) {
					let commentHeaderEl = document.createElement("h2");
					commentHeaderEl.innerText = "My Comments";
					commentsEl.appendChild(commentHeaderEl);
				}

				let commentList = document.createElement("li");

				let commentedTextEl = document.createElement("p");
				commentedTextEl.classList.add("commented");
				commentedTextEl.innerText = text;
				commentList.appendChild(commentedTextEl);

				let commenterTextEl = document.createElement("p");
				commenterTextEl.innerText = comment;
				commentList.appendChild(commenterTextEl);

				commentsEl.appendChild(commentList);

				dialogEl.close();
			});

			// shows how many characters have been used
			dialogCommentEl.addEventListener("keyup", (e) => {
				e.preventDefault();
				let text = e.target.value;
				characterEl.innerText = `Characters: ${text?.length}/50`;
			});
		</script>
	</body>
</html>
{{< /highlight >}}
