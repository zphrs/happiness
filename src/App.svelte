<script>
  import RippleCanvas from "./lib/RippleCanvas.svelte"

  import Title from "./lib/Title.svelte"
  import { onMount } from "svelte"
  import Subtitle from "./lib/Subtitle.svelte"
  import Card from "./lib/Card.svelte"
  import Quote from "./lib/Quote.svelte"
  import Toc from "./lib/toc.svelte"
  import Side from "./lib/Side.svelte"
  import ProgressBar from "./lib/ProgressBar.svelte"
  let rc
  let divs = []

  onMount(() => {
    console.log("HERE", rc)
  })
</script>

<main>
  <RippleCanvas bind:this={rc} />
  <Title rippleCanvas={rc} />
  <Card
    bind:this={divs[0]}
    delay={4}
    {rc}
    title={"What is Happiness?"}
    visible={true}
    >Happiness is a state of satisfaction, according to University of Illinois
    psychologist Edward Diener.
    <Quote
      source={"The New Science of Happiness"}
      link="https://content.time.com/time/magazine/article/0,9171,1015832-2,00.html#:~:text=Diener%20has%20created%20one%20of%20the%20most%20basic%20and%20widely%20used%20tools,%20the%20Satisfaction%20with%20Life%20Scale."
    >
      Diener has created one of the most basic and widely used tools, the
      Satisfaction with Life Scale. Though some scholars have questioned the
      validity of this simple, five-question survey, Diener has found that it
      squares well with other measures of happiness, such as impressions from
      friends and family, expression of positive emotion and low incidence of
      depression.
    </Quote>
    Ultimate satisfaction, and therefore ultimate happiness, come from moderation
    and balance, according to the virtuous, The Hero's Journey, and buddhism.
  </Card>
  <Card
    bind:this={divs[1]}
    delay={6}
    {rc}
    title={"The Virtuous Viewpoint"}
    visible={false}
  >
    Often those who are said to be virtuous are said to be happy and satisfied
    as well.
    <br />
    Virtues are all about working toward having just the right amount of any given
    trait. Ben Franklin listed out thirteen virtues in his Bibliography. Here are
    a few of the thirteen:
    <Quote link="http://thirteenvirtues.com" source={"Ben Franklin"}>
      5. Frugality. Make no expense but to do good to others or yourself; i.e.,
      waste nothing.
      <br />
      9. Moderation. Avoid extremes; forbear resenting injuries so much as you think
      they deserve.
      <br />
      11. Tranquillity. Be not disturbed at trifles, or at accidents common or unavoidable.
    </Quote>
    Franklin qualifies traits with conditions to form his list. For instance, Franklin
    defined the virtue of frugality as investing the right amount to do good but
    not more than necessary and moderation as staying calm in the face of injury.
    This shows that virtues are truly about balancing the right amount of each trait.
    <br />
    Ben Franklin also writes directly of happiness, focusing on balancing wants with
    means.
    <Quote
      link="https://www.goodreads.com/quotes/150284-there-are-two-ways-of-being-happy-we-may-either"
      source={"Ben Franklin"}
    >
      There are two ways of being happy: We may either diminish our wants or
      augment our means- either will do- the result in the same; and it is for
      each man to decide for himself, and do that which happens to be the
      easiest.
    </Quote>
    It is obvious that if we desire more then we would be less happy. It is also
    obvious that if we have more then we would be more happy. However, an important
    note is that true happiness must come from tuning our desires and using what
    we have to better society as a whole. Ben Franklin continues,
    <Quote
      link="https://www.goodreads.com/quotes/150284-there-are-two-ways-of-being-happy-we-may-either"
      source={"Ben Franklin"}
    >
      if you are very wise you will [diminish your wants and augment your means]
      in such a way as to augment the general happiness of society.
    </Quote>
  </Card>
  <Card
    bind:this={divs[2]}
    delay={8}
    {rc}
    visible={false}
    title="The Hero's Journey (to Fulfilment)"
  >
    <img src="./heroes-journey.png" alt="Hero's journey circle" />
    <p>
      The Hero's Journey, also known as the monomyth, is a pattern found by
      Joseph Campell in stories across many different cultures and eras. The
      well defined story arc was popularized by his 1949 book
      <i>The Hero with a Thousand Faces</i>. The Hero's Journey is a pattern of
      departure, adventure, and return to ultimately grow as a person.
      <br />
      In the book <i>Siddhartha</i> by Herman Hesse, Siddhartha goes through the
      hero's journey, starting with arrogance and ending with humility and
      enlightenment. The most relevent part of Siddhartha's journey is his
      return. Siddhartha desired enlightnment for many years, but he only
      attained it when he stopped desiring it and instead embraced it.
      <Quote
        link="https://www.gutenberg.org/files/2500/2500-h/2500-h.htm#:~:text=In%20this%20hour%2C%20Siddhartha%20stopped%20fighting%20his%20fate,belonging%20to%20the%20oneness."
        source="Siddhartha by Herman Hesse"
      >
        In this hour, Siddhartha stopped fighting his fate, stopped suffering.
        On his face flourished the cheerfulness of a knowledge, which is no
        longer opposed by any will, which knows perfection, which is in
        agreement with the flow of events, with the current of life, full of
        sympathy for the pain of others, full of sympathy for the pleasure of
        others, devoted to the flow, belonging to the oneness.
      </Quote>
      Soon after Siddhartha's moment of enlightenment (which would be the gift of
      the goddess in The Hero's Journey), he helps his childhood friend Govinda achieve
      enlightenment by attempting to explain the unity of the world, that suffering
      and enlightenment are one. This is Siddhartha's return home. It is not Siddhartha's
      words which enlighten his friend but rather what Govinda sees when he kisses
      Siddhartha's forehead.
      <Quote
        link="https://www.gutenberg.org/files/2500/2500-h/2500-h.htm#:~:text=He%20saw%20the%20face%20of%20a%20fish,between%20the%20one%20and%20the%20other%20face"
        source="Siddhartha by Herman Hesse"
      >
        [Govinda] saw the face of a fish, a carp, with an infinitely painfully
        opened mouth, the face of a dying fish, with fading eyes—he saw the face
        of a new-born child, red and full of wrinkles, distorted from crying—he
        saw the face of a murderer, he saw him plunging a knife into the body of
        another person—he saw, in the same second, this criminal in bondage,
        kneeling and his head being chopped off by the executioner with one blow
        of his sword—he saw the bodies of men and women, naked in positions and
        cramps of frenzied love—he saw corpses stretched out, motionless, cold,
        void—he saw the heads of animals, of boars, of crocodiles, of elephants,
        of bulls, of birds—he saw gods, saw Krishna, saw Agni—he saw all of
        these figures and faces in a thousand relationships with one another,
        each one helping the other, loving it, hating it, destroying it, giving
        re-birth to it, each one was a will to die, a passionately painful
        confession of transitoriness, and yet none of them died, each one only
        transformed, was always reborn, received evermore a new face, without
        any time having passed between the one and the other face.
      </Quote>
      When Govinda seees for himself the cycle of suffering, the cycle of life, he
      becomes enlightened. It is the embrace of everything which leads to enlightenment.
      <br />
      The book ends here, the childhood friends finally together in enlightenment,
      concluding Siddartha's journey.
      <br />
      It is only when the hero returns home with newfound knowledge from his experiences,
      balancing their new selves with the old environment, that the hero feels fulfilled.
      The Hero's Journey is fueled by the eventual balance of personal discovery
      with helping one's community.
      <br />
      Are there similar cliched arcs which focus on balancing other ethical dilemmas
      instead of the ethical dilemma between focusing on bettering oneself or bettering
      one's community?
    </p>
  </Card>
  <Card
    bind:this={divs[3]}
    delay={10}
    {rc}
    title={"The Buddist's Perspective"}
    visible={false}
  >
    Buddhism suggests that happiness is achieved through achieving sukha.
    Matthieu Ricard authored an overview of Buddhism with a focus on happiness.
    Ricard first summarizes how modern society views happiness.
    <Quote
      link="https://www.cambridge.org/core/journals/journal-of-law-and-religion/article/abs/buddhist-view-of-happiness/F1E04351F8496F5387F783865DAB7F40"
      source="A Buddhist View of Happiness by Matthieu Ricard"
    >
      In modern societies, happiness is often equated with a maximization of
      pleasure, and some imagine that true happiness would consist of an
      interrupted succession of pleasurable experiences. This sounds more like a
      recipe for exhaustion than for genuine happiness.
    </Quote>
    Ricard then notes how sukha is different to how contemporary society views happiness.
    <Quote
      link="https://www.cambridge.org/core/journals/journal-of-law-and-religion/article/abs/buddhist-view-of-happiness/F1E04351F8496F5387F783865DAB7F40"
      source="A Buddhist View of Happiness by Matthieu Ricard"
    >
      Nothing is further from the Buddhist notion of sukha, which refers to an
      optimal way of being, an exceptionally healthy state of mind that
      underlies and suffuses all emotional states and embraces all the joys and
      sorrows that come one's way. Sukha is therefore a state of lasting
      well-being that manifests itself when we have freed ourselves of mental
      blindness and afflictive emotions.
    </Quote>
    Buddhists, in contrast to contemporary society, seek peace and balance, instead
    of seeking an increasing amount of pleasure. Buddhism teaches to be happy with
    the balance of the universe rather than sway it in one's own direction. Instead
    of seeking to gain more money, Buddhists seek to match their wants with whatever
    the universe is willing to provide. They have decided to diminish their wants
    while contemporary society have decided to tirelessly work to try to increase
    our means.
    <br />
    I wonder if Buddists ever fear imposter syndrome, where they fear that they are
    not truly enlightened. I also wonder if enlightenment is like a switch, on or
    off, or if it is more accurately a continuum.
  </Card>

  <Card {rc} title="Conclusion" delay={12} bind:this={divs[4]} visible={true}>
    Ben Franklin talked of the balance involved with virtues and of balancing
    wants with means. The Hero's Journey centers on the balance between
    exploration with bettering general society. Buddhism focuses on the balance
    of desires with what the universe provides. In all these cases, a form of
    balance is integral to finding happiness.
  </Card>
  <Card {rc} title="Shout Outs" delay={14} bind:this={divs[5]} visible={false}>
    Shout out to Ms. Beare and the rest of my sixth period philosophy class. I
    love how the class increased my understanding of other people, the
    underlying motivations behind my behavior, my inner peace. Thank you all for
    making this class a fun, accepting, and educational experience.
  </Card>
</main>
<Side>
  <span slot="main">
    <Toc {divs} {rc} />
  </span>
  <span slot="under">
    <ProgressBar />
  </span>
</Side>

<style>
  img {
    width: 100%;
    max-width: 400px;
    /* center horizontally */
    position: relative;
    left: 50%;
    transform: translateX(-50%);
  }
  :root {
    --primary: 251, 238, 203;
    --secondary: 2, 65, 1;
    --lgt-secondary: 156, 169, 124;
    --lighter-secondary: 204, 203, 164;
    --accent: 235, 71, 153;
    --lgt-accent: 244, 140, 175;
    --avg: 119, 68, 77;
    --primary-color: rgb(var(--primary));
    --secondary-color: rgb(var(--secondary));
    --accent-color: rgb(var(--accent));
    --avg-color: rgb(var(--avg));
    --glass-color: rgba(var(--secondary), 0.1);
    background-color: var(--primary-color);
    color: var(--secondary-color);
    font-family: Avenir, Helvetica, Arial, sans-serif;
    position: relative;
  }
  :global(*) {
    -webkit-tap-highlight-color: transparent;
  }
  main {
    width: 100%;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
    font-size: 1.1rem;
  }
  @media screen and (min-width: 1420px) {
    main {
      margin-left: auto;
      margin-right: auto;
    }
  }

  @media screen and (max-width: 1420px) {
    main {
      margin-left: 300px;
      margin-right: 20px;
    }
  }
  @media screen and (max-width: 1120px) {
    main {
      margin-left: auto;
      margin-right: 0.25rem;
    }
  }

  :global(.glass) {
    background-color: var(--glass-color);
    padding: 0.5rem;
    z-index: -1;
    margin: 0.25rem;
    margin-left: auto;
    margin-right: auto;
    border-radius: 0.5rem;
    display: block;
    padding-top: 1rem;
    background: linear-gradient(
      90deg,
      rgba(var(--secondary), 0.1),
      rgba(var(--accent), 0.2)
    );
    text-align: left;
    /* border: 2px solid var(--secondary-color); */
  }
  @keyframes fadeout {
    from {
      backdrop-filter: blur(8px);
      background-color: rgb(var(--primary), 1);
    }
    to {
      backdrop-filter: blur(0px);
      background-color: rgb(var(--primary), 0);
    }
  }

  @keyframes stretch {
    from {
      transform: scaleX(0);
    }
    to {
      transform: scaleX(1);
    }
  }
</style>
