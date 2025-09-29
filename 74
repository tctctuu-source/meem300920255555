import React from 'react';
import { Result, PosterStyles } from '../../../types';

interface PosterLayoutProps {
  program: { event: string; category: string };
  winners: Result[];
  styles: PosterStyles;
  background_image_url: string | null;
  resultNumber: number;
}

const StyledText: React.FC<{ style: any; children: React.ReactNode; className?: string }> = ({ style, children, className }) => {
  if (!style) return null;
  
  const cssProperties: React.CSSProperties = {
    position: 'absolute',
    top: `${style.y}%`,
    left: `${style.x}%`,
    transform: 'translate(-50%, -50%)',
    width: '90%',
    fontSize: `${style.size}px`,
    color: style.color,
    textAlign: style.align,
    fontWeight: style.weight,
    fontFamily: `'${style.font}', sans-serif`,
    lineHeight: 1.2,
    textShadow: '1px 1px 3px rgba(0,0,0,0.3)',
  };

  return <div style={cssProperties} className={className}>{children}</div>;
};

const FlexibleLayout: React.FC<PosterLayoutProps> = ({ program, winners, styles, background_image_url, resultNumber }) => {
  const winner1 = winners.find(w => w.position === 1);
  const winner2 = winners.find(w => w.position === 2);
  const winner3 = winners.find(w => w.position === 3);

  const backgroundStyle: React.CSSProperties = background_image_url
    ? { backgroundImage: `url(${background_image_url})` }
    : {};

  return (
    <div 
      className="w-full h-full bg-cover bg-center text-ui-text-light font-sans relative overflow-hidden"
      style={backgroundStyle}
    >
      <div className="absolute inset-0 bg-black/30"></div> {/* Overlay for better text readability */}
      
      {styles.resultNumber && <StyledText style={styles.resultNumber}>{String(resultNumber).padStart(2, '0')}</StyledText>}
      <StyledText style={styles.programName} className="uppercase tracking-widest">{program.event}</StyledText>
      <StyledText style={styles.category}>{program.category}</StyledText>

      {winner1 && (
        <>
          <StyledText style={styles.winner1Title}>WINNER</StyledText>
          <StyledText style={styles.winner1Name} className="font-black truncate">{winner1.participant}</StyledText>
          <StyledText style={styles.winner1Team} className="truncate">{winner1.school}</StyledText>
        </>
      )}

      {winner2 && (
        <>
          <StyledText style={styles.winner2Title}>2nd Place</StyledText>
          <StyledText style={styles.winner2Name} className="truncate">{winner2.participant}</StyledText>
        </>
      )}

      {winner3 && (
        <>
          <StyledText style={styles.winner3Title}>3rd Place</StyledText>
          <StyledText style={styles.winner3Name} className="truncate">{winner3.participant}</StyledText>
        </>
      )}
    </div>
  );
};

export default FlexibleLayout;
